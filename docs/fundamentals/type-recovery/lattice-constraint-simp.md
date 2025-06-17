# Constraint Simplification
## Introduction
Lattice based type recovery is a decompilation method for finding types in IR.
There have been 2 main attempts at this TIE[^2] and Retypd[^1].
Retypd was written as an improved approach to TIE due to supporting recursive and polymorphic types.

![Simple Lattice](/static/img/lattice.svg)

An edge in the lattice points from a supertype to its subtype e.g. int32 is a subtype of num32.
TIE works by applying constraints to infer upper and lower bounds on the lattice in order to output a type. 
Retypd instead chooses to represent constraints by using a sketch[^1] which is a a tree where the nodes are types and the outgoing edges are labelled by capabilities of that type. 

The following article will focus on constraint generation and simplification this can be represented by F.1 from the Retypd paper[^1].
[Retypd](https://github.com/GrammaTech/retypd) has been open-sourced and is actively used in decompilers such as [angr](https://github.com/angr/angr/blob/master/angr/analyses/typehoon/typehoon.py).
The article focuses mainly on the Retypd implementation which diverges slightly as described [here](https://github.com/GrammaTech/retypd/blob/master/reference/type-recovery.rst) in the type recovery outline.

## Prerequisites
This section will cover some of the basics of type theory and other concepts contained within the Retypd paper[^1].

### Subtypes
Written A <: B means that A is a subtype of B.
This is commonly found in OOP languages, for example if we declare an interface of Animal and a class Dog that invokes this we state Dog <: Animal.

_You may also see A <: B written as A ⊆ B._

### Labels
Labels are used in the paper to denote the capabilities of a type (denoted type.label). 
A pointer of type X could have 3 capabilities:

1. X.load
2. X.store
3. X.σN@k

X.load is equivalent to dereferencing a pointer (A = \*X). 
X.store is equivalent to dereferencing and setting the value of a pointer (\*X = A). 
Finally X.σN@k can easily be explained using a struct.

```C
struct Test {
    int64_t a;
    int16_t b;
};

struct Test* t;
t->b;
```

t->b equates to t.σ16@64 meaning we read 16 bits at the offset 64.
If we assigned t.b to a variable, we could then say t has the capability of t\.load\.σ16@64.

Functions also have 2 labels:

1. inL 
2. outL

These simply refer to the input parameters and return at location L respectively.

### Variance
Variance is a way of describing the relationship between labels of types based on the subtyping of the root types. 
There is 2 types of variance:

1. Covariant (**out, load and σN@k**)
2. Contravariant (**in and store**)

Covariant means that if A <: B then A.label <: B.label. 
Contravariant is the reverse i.e. if A <: B then B.label <: A.label.
 
### Deduction Rules
Deduction rules are a way of drawing conclusions from a set of statements know as premises.
We write it in the following form:

$$
\frac{P_1 \; P_2 \; \ldots \; P_3}{C}
$$

This is useful because we can write our covariant and contravariant definitions using it.
\+ is used for covariant and - is for contravariant. 

Covariant:

$$
\frac{A <: B \quad \text{Var} \; B.\text{label} \quad \langle \text{label} \rangle = +}{A.\text{label} <: B.\text{label}}
$$

Contravariant:

$$
\frac{A <: B \quad \text{Var} \; B.\text{label} \quad \langle \text{label} \rangle = -}{B.\text{label} <: A.\text{label}}
$$

For our implementation we implicitly use these rules over the simplification process discussed in the following sections.

## Generating Constraints
For this stage we start with our lifted IR program and initial constraints and variables which can be found by identifying parameters for the functions and matching patterns from IR statements/expressions.
Constraints are generated based on the usage of the identified variables.
The following usages are described in appendix A of the Retypd paper[^1]:

1. Register Loads and Stores
2. Addition and Subtraction
3. Memory Loads and Stores
4. Procedure Invocation
5. Floating Point
6. Bit Manipulation

### Examples
The goal of these examples is to show how the rules found in appendix A are used.

| Constraint Type | Input | Output |
| -------- | ------- | -------- |
| Register Loads and Stores | MOV R1, R2 | R2 <: R1 |
| Addition and Subtraction (const) | var = A + const | A.+const <: var |  
|  |  var = A - const | A.-const <: var |  
| Addition and Subtraction (no const) | var = A + B | Add(B, C, A) |  
|  | var = A - B | Sub(B, C, A) |  
| Memory Loads and Stores | A = \*B | B.load.σk@0 <: A |
|  | \*A = B | B <: A.store.σk@0 |
| Procedure Invocation | A = F(B) | F.out <: A<br> B <: F.in |

Floating point is a special case as you can choose to implement them as explicit constraints or only in the circumstance of known function inputs/outputs. 
This is similar for bit manipulation which is largely ignored except for some special circumstances[^1].

You may have noticed that addition and subtraction do not follow the expected pattern when a constant value cannot be evaluated.
This is done to propagate information throughout constraint generation process to determine if a variable is a pointer or an integer, since these operations can be applied to both. 
We discuss how we use them later.

## Inferring Sketch Shape
### Collecting Constraints
The goal of this is to take the constraint set that we have generated over strongly connected components (SCCs) in the call graph of our program bottom-up. 
This is done by creating a union of the constraints generated from each procedure contained within a SCC.

The instantiate procedure replaces references to previously computed procedure calls. 
In other words when we come across a F.inL or an F.outL we replace these constraints with new ones containing explicit types from our lattice.
To support Retypd's polymorphic properties you have to treat each set of inputs and outputs as unique for each instance of a function call.
This can be done by computing the output constraint by applying the input constraints.

Once our constraint set has been created we can now infer the shapes of our sketches.

### Inferring Sketch Shape
The inference of a sketch shape is simply assigning the edges (labels) of our sketch but not assigning exact types to the nodes just yet. 
The following uses pseudocode snippets from Algorithm E.1[^1] to aid with the explanation.

```
G ← ∅ 
for all p.L1 ... Ln ∈ C.derivedTypeVars do
    for i ← 1 ... n do
        s ← FINDEQUIVREP(p.`1 ... `i−1, G)
        t ← FINDEQUIVREP(p.`1 ... `i, G)
        G.edges ← G.edges ∪ (s, t, `i)
    end for
end for
```
Our first step is the compute a quotient graph which starts with a node for each derived type variable (a type variable with labels) represented by an equivalence node.
A derived type variable A.B.C becomes A->A.B and A.B->A.B.C in our graph.
A->A.B would have the label .B and A.B->A.B.C would have the label .C.

```
for all x <: y ∈ C do
    X ← FINDEQUIVREP(x, G)
    Y ← FINDEQUIVREP(y, G)
    UNIFY(X, Y, G)
end for
```
Our second step should be self explanatory, except for UNIFY which we will cover below.

```
repeat
    C_old ← C
    for all c ∈ C_old with c = ADD(_) or SUB(_) do
        D ← APPLYADDSUB(c, G, C)
        for all δ ∈ D with δ = X <: Y do
            UNIFY(X, Y)
        end for
    end for
until C_old = C
```
Our third step is where we apply our Add and Sub constraints.
These constraints are used to propagate whether a variable is a pointer or not detailed in Figure 13[^1].
Figure 13 contains both inferred and explicit types that can be evaluated to create new subtypes.
By traversing the edges in our graph G from our result variable, labels applied at the edges such as .load and .store (which indicate a pointer) can allow us to determine the potential output types of the addition explicitly or implicitly.

```
for all v ∈ C.typeVars do
    S ← new Sketch
    L(S) ← ALLPATHSFROM(v, G)
    for all states w ∈ S do
        if <w> = + then
            νS(w) ← T
        else
            νS(w) ← ⊥
        end if
    end for
    B[v] ← S
end for
```
This creates our initial sketches.
We assign T or ⊥ dependent on if all out going labels are covariant or contravariant.
T represents the lowest order of our lattice, and ⊥ represents the highest i.e. T can be any type and ⊥ is an inconsistent type[^2].
```
procedure UNIFY(X, Y, G)
    if X != Y then
        MAKEEQUIV(X, Y, G)
        for all (X', l) ∈ G.outEdges(X) do
            if (Y', l) ∈ G.outEdges(Y) for some Y' then
                UNIFY(X0, Y0, G)
            end if
            else if l = .load and (Y', .store) ∈ G.outEdges(Y) for some Y' then
                UNIFY(X0, Y0, G)
            end else if
        end for
    end if
end procedure
```
This procedure simply combines 2 equivalent nodes by merging them.
Any outgoing labels that are the same or a subtype relation of .load <: .store should also cause their targets to recursively call unify.
This behaviour is shown in the figure below:

![](/static/img/unify.svg)

## Creating a Pushdown System
This is where we simplify our constraints.
A pushdown system is a model to traverse a constraint graph by using a stack.
The model is derived from the deduction rules defined in Figure 3[^1].
To use the model we first define a start (F.in0) and an end point (F.out) of our sketch.
Then we define a constraint graph.

### Identifying Interesting Constraints
The goal with identifying interesting constraints is to find constraints which we can apply a set of heuristics against in order to create a simplified constraint set. 
This simplified constraint set increases the speed and conciseness in which we can solve our constraints later.
If a constraint has one of the following properties it is considered interesting:

1. A capability constraint.
2. A recursive subtype constraint.
3. A subtype constraint containing a type constant.

### Generating the Constraint Graph
For each subtype relation (A <: B and A.load <: C.store.σN@k). in our initial constraint set we create the edges seen in the figure below. 
It is important that these edges remain unlabelled.

![](/static/img/cg1.svg)

We then add a set of edges with the labels push or pop (forget and recall in the source code) for both sets of nodes.

1. From the subtype node we recursively create pop edges to the prefixes and also returning push edges maintaining correct variance.
2. From the supertype node we recursively create push edges to the prefixes and also returning pop edges maintaining correct variance.

![](/static/img/cg2.svg)

### Saturating
When saturating we wish to add shortcut edges to push and pop sequences. 
The algorithm in Retypd is an adaptation of Caucal's[^3] and has been evaluated as being O(n³) A.K.A. Big-Oh-No.

To allow the skipping of push and pops that negate one another, we start by adding edges for nodes that meet the following pattern:

1. There is a sequence of our unlabelled edges and one labelled push edge L from p to q. 
2. If q also has a pop edge with label L to another node q'.
3. Then create a new unlabelled edge from p to q'.

Retypd also handles the implicit rule A.load <: A.store.
This is done after the above modifications.
We then add a few finishing touches by doing the following:

1. Remove self-loops.
2. Make sure no pop edges are reachable after traversing a single push edge.

### Breaking SCCs
To be able to identify recursive type information we break SCCs.
This is done by removing the existing interesting nodes and then identifying the set of SCCs.
For the SCCs we break them by removing a node contained within that SCC with the shortest prefix and adding it to the set of interesting nodes.
We stop once there is no more SCCs.

## Solving Constraints
For the function we analysed we can now solve the constraints using our simplified constraint graph.
The Retypd implementation provides several solvers for constraints viewable [here](https://github.com/GrammaTech/retypd/blob/master/src/graph_solver.py).
The next article discusses how to solve constraints given our simplified graph.


## Code Examples
N.B. Both the official Retypd implementation and angr deviate from the original paper.

- [Retypd](https://github.com/GrammaTech/retypd/tree/master)
- [angr Typehoon](https://github.com/angr/angr/blob/master/angr/analyses/typehoon/typehoon.py)

[^1]: Noonan, Matt, Alexey Loginov, and David Cok. "Polymorphic type inference for machine code." Proceedings of the 37th ACM SIGPLAN Conference on Programming Language Design and Implementation. 2016.
[^2]: Lee, JongHyup, Thanassis Avgerinos, and David Brumley. "TIE: Principled reverse engineering of types in binary programs." 2011.
[^3]: Caucal, Didier. On the regular structure of prefix rewriting. Theoretical Computer Science, 106(1):61–86, 1992.