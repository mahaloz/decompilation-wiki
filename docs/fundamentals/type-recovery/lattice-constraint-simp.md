# Constraint Simplification
## Introduction
*Lattice-based type recovery* is a decompilation technique that infers data types in an intermediate representation (IR).
Two major systems use this approach: **TIE** [^2] and **Retypd** [^1].
Retypd extends TIE by supporting both recursive and polymorphic types.

![Simple Lattice](/static/img/lattice.svg)

In a type lattice, an edge points from a supertype to its subtype—e.g., `int32` is a subtype of `num32`.
TIE determines upper and lower lattice bounds by applying constraints and then chooses an appropriate type.
Retypd instead encodes constraints in a *sketch* [^1]: a tree whose nodes are types and whose outgoing edges are labeled with the capabilities of those types.

This article outlines Retypd’s process for constraint generation and simplification (see Figure 1 in [^1]).
Retypd is [open-source](https://github.com/GrammaTech/retypd) and underpins decompilers such as [**angr**](https://github.com/angr/angr/blob/master/angr/analyses/typehoon/typehoon.py).
The implementation diverges slightly from the original publication; those differences are summarized in the [type-recovery outline](https://github.com/GrammaTech/retypd/blob/master/reference/type-recovery.rst)

---

## Prerequisites
This section reviews the key type-theory terms used in lattice-based constraint simplification[^1].

### Subtypes
The notation `A <: B` means “*A* is a subtype of *B*.”
Many object-oriented languages adopt the same idea—for example, if `Dog` implements `Animal`, then `Dog <: Animal`.

*Alternate form*: some texts write the relation as `A ⊆ B`.

### Labels
A **label** describes a capability of a type and is written `type.label`.
For a pointer of type `X`, Retypd models three capabilities:

1. `X.load`
2. `X.store`
3. `X.σN@k`

`X.load` reads through the pointer (`A = *X`).
`X.store` writes through the pointer (`*X = A`).
`X.σN@k` accesses *N* bits at offset *k*; for instance, `t->b` in the code below is represented as `t.σ16@64`.

```c
struct Test {
    int64_t a;
    int16_t b;
};

struct Test *t;
t->b;
```

If the program assigns `t->b` to a variable, then `t` acquires the capability `t.load.σ16@64`.

Functions introduce two additional labels:

| Label  | Meaning                      |
| ------ | ---------------------------- |
| `inL`  | Parameters at location *L*   |
| `outL` | Return value at location *L* |

### Variance

*Variance* explains how a label’s subtyping relationship follows (or reverses) the relationship of its base type:

| Kind              | Labels                | Rule                                    |
| ----------------- | --------------------- | --------------------------------------- |
| **Covariant**     | `out`, `load`, `σN@k` | If `A <: B`, then `A.label <: B.label`. |
| **Contravariant** | `in`, `store`         | If `A <: B`, then `B.label <: A.label`. |

### Deduction Rules

Retypd represents inference rules in sequent form:

$$
\frac{P_1 \quad P_2 \quad \dots \quad P_n}{C}
$$

Using `+` for covariance and `−` for contravariance gives:

**Covariant**

$$
\frac{A <: B \quad \text{Var}\,B.\text{label} \quad \langle \text{label} \rangle = +}
     {A.\text{label} <: B.\text{label}}
$$

**Contravariant**

$$
\frac{A <: B \quad \text{Var}\,B.\text{label} \quad \langle \text{label} \rangle = -}
     {B.\text{label} <: A.\text{label}}
$$

Retypd applies these rules implicitly during the later simplification pass.

---

## Generating Constraints

Constraint generation starts with the lifted IR plus an initial set of variables identified from function parameters and instruction patterns.
Appendix A of [^1] groups variable uses into six categories:

1. Register loads and stores
2. Addition and subtraction
3. Memory loads and stores
4. Procedure invocation
5. Floating-point operations
6. Bit manipulation

### Examples

The table below shows how each IR pattern translates to a constraint.

| Category / IR Input | Constraint Produced            |
| ------------------- | ------------------------------ |
| `MOV R1, R2`        | `R2 <: R1`                     |
| `var = A + const`   | `A.+const <: var`              |
| `var = A − const`   | `A.-const <: var`              |
| `var = A + B`       | `Add(B, C, A)`                 |
| `var = A − B`       | `Sub(B, C, A)`                 |
| `A = *B`            | `B.load.σk@0 <: A`             |
| `*A = B`            | `B <: A.store.σk@0`            |
| `A = F(B)`          | `F.out <: A`  and  `B <: F.in` |

Floating-point and bit-manipulation instructions may be handled explicitly or only when function signatures reveal the need [^1].
Addition and subtraction propagate pointer-versus-integer information even when the constant operand is unknown, as explained in a later section.

---

## Inferring Sketch Shape

### Collecting Constraints

Retypd processes strongly connected components (SCCs) of the call graph from the leaves upward:

1. Merge the constraints produced by every procedure in an SCC.
2. *Instantiate* each call site: replace symbolic labels such as `F.inL` or `F.outL` with concrete lattice types, treating every call polymorphically.

### Building the Quotient Graph

Algorithm E.1 [^1] constructs a *quotient graph* `G` whose nodes are equivalence classes of derived type variables.

```pseudo
G ← ∅
for p.L1 … Ln in C.derivedTypeVars do
    for i ← 1 … n do
        s ← FINDEQUIVREP(p.`1 … `i−1, G)
        t ← FINDEQUIVREP(p.`1 … `i,   G)
        G.edges ← G.edges ∪ (s, t, `i)  // label `i`
    end for
end for
```

For example, the variable `A.B.C` yields edges `A → A.B` (labeled `.B`) and `A.B → A.B.C` (labeled `.C`).

### Unification

Each explicit subtype constraint `x <: y` is enforced by unifying the corresponding nodes:

```pseudo
for x <: y in C do
    X ← FINDEQUIVREP(x, G)
    Y ← FINDEQUIVREP(y, G)
    UNIFY(X, Y, G)
end for
```

`UNIFY` merges equivalence classes and recurses on matching edges or the compatible pair `.load`/`.store`:

```pseudo
procedure UNIFY(X, Y, G)
    if X ≠ Y then
        MAKEEQUIV(X, Y, G)
        for (X', l) in G.outEdges(X) do
            if (Y', l) in G.outEdges(Y) then
                UNIFY(X', Y', G)
            else if l = .load and (Y', .store) in G.outEdges(Y) then
                UNIFY(X', Y', G)
            end if
        end for
    end if
end procedure
```

![Unification Example](/static/img/unify.svg)

### Propagating `Add` / `Sub`

Retypd evaluates each `ADD` or `SUB` constraint repeatedly:

```pseudo
repeat
    C_old ← C
    for c in C_old with c = ADD(_) or SUB(_) do
        D ← APPLYADDSUB(c, G, C)   // may generate new subtype edges
        for δ in D with δ = X <: Y do
            UNIFY(X, Y, G)
        end for
    end for
until C_old = C
```

Figure 13 of [^1] shows how labels such as `.load` and `.store` disclose pointer information during this phase.

### Initial Sketches

Once propagation converges, each type variable receives an initial sketch:

```pseudo
for v in C.typeVars do
    S ← new Sketch
    L(S) ← ALLPATHSFROM(v, G)
    for state w in S do
        νS(w) ← (⟨w⟩ = +) ? T : ⊥   // T = lattice bottom; ⊥ = lattice top
    end for
    B[v] ← S
end for
```

---

## Creating a Pushdown System

The next step simplifies the constraint set with a pushdown system (PDS) derived from Figure 3 of [^1].
`F.in0` serves as the start state and `F.out` as the final state.

### Interesting Constraints

A constraint is *interesting* if it satisfies any of these conditions:

1. It involves a capability label.
2. It is recursively self-referential.
3. It contains a type constant.

### Constraint Graph Construction

For every subtype edge (e.g., `A <: B`, `A.load <: D` and `C <: B.store`) the PDS adds an **unlabeled** edge:

![Initial Constraint Graph](/static/img/cg1.svg)

Next, two families of labeled edges are inserted:

| Direction          | Edge label | Description                             |
| ------------------ | ---------- | --------------------------------------- |
| Subtype → prefix   | `pop`      | Remove one label from the end (forget). |
| Supertype → prefix | `push`     | Add one label to the end (recall).      |

Edges are added recursively to cover all prefixes, maintaining correct variance:

![Add push/pop edges](/static/img/cg2.svg)

### Saturation

Caucal’s algorithm [^3] adds shortcut edges so consecutive `push`/`pop` pairs cancel, achieving closure in $O(n^3)$ time. Additional adjustments:

1. Insert edges implied by `A.load <: A.store`.
2. Remove self-loops.
3. Ensure a `pop` edge is unreachable after a single `push` with the same label.

![Saturation](/static/img/cg3.svg)

In the example above, the following path:
```
A.load.⊖
     → A.store.⊖ (implied node)
push → A.⊕ 
     → B.⊕
pop  → B.store.⊖
```

This path contains a `push` directly followed by a `pop`.
Where both use the same label: `store`.
Therefore, we can apply the shortcut:

```
A.load.⊖ → B.store.⊖
```

### Breaking SCCs

Recursive type information becomes visible by iteratively removing strongly connected components:

1. Temporarily strip the current set of interesting nodes.
2. Identify remaining SCCs.
3. For each SCC, select the node with the shortest label prefix, add it to the interesting set, and remove it from the graph.
4. Repeat until no SCCs persist.

---

## Code Examples
The Retypd implementation provides several solvers for constraints viewable [here](https://github.com/GrammaTech/retypd/blob/master/src/graph_solver.py).

Although the Retypd implementation and angr’s **Typehoon** analysis follow the same principles, both deviate slightly from the original paper:

* [Retypd source tree](https://github.com/GrammaTech/retypd/tree/master)
* [angr Typehoon](https://github.com/angr/angr/blob/master/angr/analyses/typehoon/typehoon.py)

---

[^1]: Noonan, Matt; Loginov, Alexey; and Cok, David. “Polymorphic Type Inference for Machine Code.” *PLDI 2016*.
[^2]: Lee, JongHyup; Avgerinos, Thanassis; and Brumley, David. “TIE: Principled Reverse Engineering of Types in Binary Programs.” *USENIX Security 2011*.
[^3]: Caucal, Didier. “On the Regular Structure of Prefix Rewriting.” *Theoretical Computer Science* 106 (1): 61–86, 1992.