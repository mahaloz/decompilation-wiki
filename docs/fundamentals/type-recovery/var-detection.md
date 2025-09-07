# Variable Detection
## Introduction
Variable detection is a collection of techniques used to annotate memory regions and registers with variable type classifications. 
Common classifications of variables include: global, local, heap and register. 
The output from variable classification and detection provides the basis for further analysis and can greatly impact the quality of decompilation.

## Naïve Variable Identification
Early decompilation[^1] primarily focused on identifying common idioms in the assembly:

| Type   | Access Pattern                   |
| ------ | -------------------------------- |
| Local  | [rbp-4]                          |
| Global | [0xdeadbeef] or [rip+0xdeadbeef] |

Local variables are typically identified as they are accessed as an offset from the frame pointer.
Global variables are typically accessed absolutely, this can be done via an exact address or another common way is as an offset from the instruction pointer.
We can further this analysis by handling calling conventions, in order to allow the detection of arguments and return addresses.

Calling conventions allow the interoperation of multiple languages by defining:

- How arguments are passed (by stack or register).
- Who should preserve registers (caller or callee).
- How values are returned.

The standardisation introduced by calling conventions can be applied via pattern matching in order to detect function information and therefore variables.

This simple identification of variables is surprisingly accurate detecting 83% of local variables[^2]. 
However it has a few limitations because variables can be accessed indirectly, promoted to registers or allocated on the heap.
Despite this many decompilers rely on it as the foundation for their variable and type analysis due to its efficiency, accuracy and simplicity.

## DIVINE
DIVINE[^2] introduced the idea of used a more advanced approach by combining Value-Set Analysis (VSA) with Aggregate Structure Identification.
VSA analyses and models memory to understand potential addresses and values of memory.
Aggregate Structure Identification (ASI) uses the information to detect data structures such as array, records or objects.
The information from ASI is then fed back into VSA if more iterations are determined to be beneficial.

## Value-Set Analysis (VSA)
VSA technique enables the identification of variables allocated on the heap and shows an improvement on the naïve techniques.
To achieve this the memory is split into global, activation record and heap regions.

### Memory Representation

VSA computes an over-approximation of memory regions, done by maintaining a set of possible values or addresses for a memory region.
Memory regions are split into 3 possible categories:

| Region Type | Occurrence                |
| ----------- | --------------------------|
| Procedure   | 1 per procedure           |
| Global      | 1                         |
| Heap        | 1 per heap allocation     |

We do this because memory addresses can be reused across procedures and heap variables, however it is unlikely to be the case for global variables.
An address in a program could therefore be defined as:

```C
struct Addr {
    struct MemoryRegion region;
    size_t offset;
}; 
```

(The offset in procedure regions is represented as an offset from the base pointer).

The above is sufficient for representing the direct location of a data object, and will enable us to gain simple view of base addresses and sizes of local and global variables.
However, this unfortunately fails for indirect accesses to memory locations, so an offset for these addresses is better represented as a set of possible values.
DIVINE suggests using a strided interval: `s[l, u]`. 
Where s is the stride, and l and u are the lower and upper bounds respectively.

e.g. `4[8, 22] => {8, 12, 16}`

```C
struct Stride {
    size_t stride;
    size_t lower;
    size_t upper;
}

struct IndirectAddress {
    struct MemoryRegion region;
    size_t offset;
    size_t size;
    struct Stride value_set;
}; 
```

It is easy to see how this would be useful for representing memory offsets in an array.

To handle the indirect addressing we must be able to represent the possible values registers contain at each program point, as well as possible values of memory locations.
To represent the possible values of memory locations we use an abstract location (also known as an a-loc).
Naïve variable identification will give us the first set of abstract locations along with one abstract location per heap region and one per register.

An abstract store is a method for over-approximating the set of addresses each abstract location holds at a particular program point.
We over-approximate the potential values using a Reduced Interval Congruence (RIC) where (a,b,c,d) is equivelent to a x [b, c] + d e.g (3, 4, 7, 10) becomes {22, 25, 28, 31}.
This enables us to detail accesses in arrays or in records.

Value sets represented by the RICs have several operations that can be performed on them detailed in the DIVINE paper[^2].

```C
struct RIC {
    size_t a;
    size_t b;
    size_t c;
    size_t d;
}

struct AbstractLocation {
    size_t offset;
    size_t size;
    struct RIC value_set;
}; 

struct Store {
    struct MemoryRegion region;
    map[offset] => AbstractLocation;
}
```

### VSA Algorithm
This algorithm is able to determine a set of possible address and numeric integer values each abstract location can hold at a program point.
To perform it we create a control flow graph where each individual instruction is a node and edges link them together.
We initialise the abstract store with global variables and the stack point value.
On specific instructions we define transfer functions to model changes in the abstract store.

Instructions of interest include:

- mov
- lea
- cmp
- conditional jumps
- push
- pop

One issue is cycles, as we don't want to repeatedly analyse the same information. This can be dealt with by approximating the values that are possible known as widening. e.g. 1,3,5,7 becomes the strided interval 2[1,∞]. 
(We will discuss how to handle the infinity later).

We also want to be able to analyse between functions, this is more tricky as we need to be able to determine the parameters that are being passed to a function, which varies based on the specific calling convention.
To be able to do this we can use the value sets of registers and also add information about how push and pop modify memory.
To perform analysis between functions we must add in new nodes to represent calls and returns where the edges contain the transfer functions to represent the changes in the stack pointer and the parameters.

To populate the parameters in the case of stack values we simply look up the address from the callers memory region and identify what the possible values and addresses are, this is possible due to us adding monitoring for push and pop.
For register based parameters the same can be done by looking at the registers memory region and identifying its possible values and addresses.
We then populate each parameters value set in the abstract store by performing a join on all the value sets from the all the parameters from the callers as calculated discussed.
In the case where we identify one of the parameters to be a pointer its value set is initialised as any value.

On exit of a function we also handle reverting the changes made to the abstract store by handling restoration of registers and stack values.

Another issue we may now face is the consideration of indirect calls and jumps. 
In most cases our value sets will give us a good idea of the potential addresses, and when this cannot be determined we ignore the jump/call.

### Affine Relations
A lack of precision is introduced with loops, this is because if we don't bound our analysis it could potentially go on forever trying to calculate value sets.
To solve this we use a technique called widening which essentially identifies repeating patterns in the modification to our value set so we don't have to manually calculate each iteration (which is potentially infinite).
However with widening there is a possibility of over approximation especially in cases where the value set increases in an unbounded manner e.g. {1,3,5,7,...}.

To improve the precision of analysis and widening we use something called affine relation analysis[^5].
Affine relations are written in the following form:

$$
a_0 + \sum_{i=1}^n a_i r_i = 0
$$

This creates a way of using bounds from other registers to infer bounds on another register.

Example (set every element in an array):
```ASM
    xor ecx, ecx
    mov edx, arr
L1:
    lea eax, [ecx*8]
    add eax, edx
    mov [eax], ecx
    inc ecx
    cmp ecx, 10
    jl L1
```

In this example eax's affine relation would be:
```
eax = ecx*8 + edx
```

and since we know ecx is less than 10 at any point it is applied to eax, we can insert the values in order to solve:
```
eax = 8[0, 9] + &arr[0] = {0, 8, ..., 72} + &arr[0]
```

### Call Strings

Call strings[^7] are the method used to perform interprocedural analysis for DIVINE, they were added later since they improve accuracy. 
A call string can be defined as a path within a call graph, it aims to provide context to how we got to a particular program point.
This allows a more precise model this is because the abstract store of the callee will be specific to the current call string.
On the other hand context sensitivity is a more intensive analysis as we need to analyse each function once per valid call string.

In the below example if the set of valid paths is defined as 

`{{1,2,4}, {1,2,3}, {1,3,4}}`

This means that we analyse each of these individually, and we also are able to determing `1->2->3->4` is not a valid path and doesn't need considering.

![](/static/img/call-string.svg)

Call strings also require bounding due to large paths and especially recursive functions.

## Aggregate Structure Identification (ASI)

The output from VSA will provide us the following:

- Value sets for different types of memory (register, global, stack, and heap)
- Indirect accesses to memory
- How memory changes from one program point to another

We wish to identify variables, arrays and structs so we use ASI.

First we take the separate memory regions, analysing each of them separately.
For example in the case of a stack we can identify its size as how much memory is allocated by the stack pointer. 
(`sub esp, 80`) allocates 80 bytes of memory which defines the size of our stack region.
We break this up into smaller elements (named atoms) by using accesses to the memory:

```ASM
    mov ebp, esp
    sub esp, 80
    xor ecx, ecx
    mov edx, ebp
L1:
    lea eax, [ecx*8]
    add eax, edx
    mov [eax], ecx
    inc ecx
    cmp ecx, 10
    jl L1
    mov esp, ebp
```

From the above example we know the following value sets for the memory access `mov [eax], ecx`:

- eax: 8[ebp, ebp+72]
- ecx: 1[0, 9]

This allows us to break the 80 byte stack region into 10 and since it is using a pattern similar to accessing an array at 8 byte intervals we can define it as a array of length 10 and element size 8.
If we were seeing values in a structure being set it would likely be as constant offsets from a pointer:

```ASM
    mov [eax], 1
    mov [eax+4], 2
    mov [eax+16], 3
```

The results from ASI can be replaced back into VSA where each atom represents an abstract location, this is useful when byte boundries change from the previous set of abstract locations.
The full details of the ASI algorithm can be found in the DIVINE paper[^2].

After completing as many rounds as deemed neccessary the information can be passed to the type analysis phase.

## Improvements

One major issue with VSA and the additional affine relation analysis is its runtime[^2][^6].
The runtime is especially significant when the binary in multiple iterations of VSA and ASI.
Another is its lack of handling self modifying code.

TIE's type system[^4] uses an adaptation of VSA called DVSA which passes variables to the type analysis phase.
It unfortunately doesn't discuss runtime of the analysis.
DVSA provides several improvements over DIVINE such as:

- Uses IR in SSA form where each register assignment and memory store is unique. This helps to improve the accuracy of value sets.
- Uses linear combinations of scalars over strided intervals

SecondWrite[^3] also suggests some modifications for improving the runtime of DIVINE.

[^1]: Cifuentes, Cristina. Reverse compilation techniques. Queensland University of Technology, Brisbane, 1994.
[^2]: Balakrishnan, Gogul, and Thomas Reps. "Divine: Discovering variables in executables." International Workshop on Verification, Model Checking, and Abstract Interpretation. Berlin, Heidelberg: Springer Berlin Heidelberg, 2007.
[^3]: ElWazeer, Khaled et al. "Scalable Variable and Data Type Detection in a Binary Rewriter." PLDI '13: Proceedings of the 34th ACM SIGPLAN Conference on Programming Language Design and Implementation. 2013.
[^4]: Lee, JongHyup, Thanassis Avgerinos, and David Brumley. "TIE: Principled reverse engineering of types in binary programs." (2011)
[^5]: M¨uller-Olm, Markus and Helmut Seidl. "Precise Interprocedural Analysis through Linear Algebra." POPL '04. 2004.
[^6]: Balakrishnan, Gogul, and Thomas Reps. Analyzing memory accesses in x86 executables. Comp Construct. 2004.
[^7]: M. Sharir and A. Pnueli. "Two approaches to interprocedural data flow analysis". In Program Flow Analysis: Theory and Applications, chapter 7, pages 189–234. Prentice-Hall, 1981.