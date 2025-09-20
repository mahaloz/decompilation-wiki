# Variable Detection
## Introduction
Variable detection encompasses a set of techniques used to annotate memory regions and registers with variable type classifications.  
Common classifications include **global**, **local**, **heap**, and **register** variables.  
Accurate variable classification is foundational to subsequent analyses for decompilation.

## Naïve Variable Identification
Early decompilation techniques[^1] primarily relied on recognizing common idioms in assembly code:

| Type   | Access Pattern                   |
| ------ | -------------------------------- |
| Local  | `[rbp-4]`                        |
| Global | `[0xdeadbeef]` or `[rip+0xdeadbeef]` |

Local variables are typically identified by their access as an offset from the frame pointer.  
Global variables, in contrast, are accessed via absolute addressing—either through a fixed address or an offset relative to the instruction pointer.

This analysis can be extended by incorporating knowledge of **calling conventions**, which define:

- How arguments are passed (via stack or registers)  
- Which registers must be preserved (caller- vs. callee-saved)  
- How return values are provided  

The regularity imposed by calling conventions allows the use of pattern matching to infer function arguments, return values, and therefore variables.

Although this approach is relatively simple, it achieves surprisingly high accuracy—detecting approximately **83% of local variables**[^2].  
Its main limitations arise when variables are accessed indirectly, promoted to registers, or dynamically allocated on the heap.  
Nevertheless, many decompilers use naïve identification as the foundation for variable and type analysis because of its efficiency, simplicity, and robustness.

## DIVINE
DIVINE[^2] introduced a more advanced approach by combining **Value-Set Analysis (VSA)** with **Aggregate Structure Identification (ASI)**.  
VSA models memory to determine potential addresses and values of memory locations, while ASI detects higher-level data structures (e.g., arrays, records, objects).  
Results from ASI can be fed back into VSA for further refinement through additional iterations.

## Value-Set Analysis (VSA)
VSA enables the identification of heap-allocated variables and generally outperforms naïve techniques.  
It partitions memory into three regions: global memory, the activation record (stack), and the heap.

### Memory Representation
VSA computes an **over-approximation** of memory regions by maintaining a set of possible addresses or values for each location.  
Memory regions are typically represented as:

| Region Type | Occurrence                |
| ----------- | --------------------------|
| Procedure   | One per procedure          |
| Global      | One per program           |
| Heap        | One per heap allocation   |

This separation allows addresses to be disambiguated across procedures and heap allocations, which is generally unnecessary for globals.  

A memory address can be modeled as:

```c
struct Addr {
    struct MemoryRegion region;
    size_t offset;
};
````

Here, `offset` represents the displacement within the region (e.g., relative to the frame pointer for procedure regions).

While this representation captures direct addresses, it struggles with indirect memory accesses.
Offsets are therefore more accurately represented as **sets of possible values**, often encoded as **strided intervals** `s[l, u]`, where `s` is the stride and `[l, u]` the lower and upper bounds:

Example:
`4[8, 22] → {8, 12, 16, 20}`

This model is especially effective for representing array element offsets.

To handle indirect addressing, we must track the possible values of registers and memory at each program point.
These are represented as **abstract locations** (a-locs).
Naïve variable identification provides an initial set of a-locs: one per global, one per heap region, and one per register.

```c
struct Stride {
    size_t stride;
    size_t lower;
    size_t upper;
};

struct AbstractLocation {
    size_t offset;
    size_t size;
    struct Stride value_set;
};

struct Store {
    struct MemoryRegion region;
    map[offset] => AbstractLocation;
};
```

### VSA Algorithm

The VSA algorithm determines the set of possible addresses and values for each abstract location at every program point.
Key steps:

1. **Build a Control Flow Graph (CFG):**
   Each instruction becomes a node, with edges representing control flow.

2. **Initialize the Abstract Store:**
   Start with known global variables and initial stack/register state.

3. **Apply Transfer Functions:**
   For instructions such as `mov`, `lea`, `cmp`, `push`, `pop`, and conditional jumps, update the abstract store accordingly.

4. **Handle Cycles with Widening:**
   Repeated analysis of loops is prevented through *widening*, which generalizes patterns.
   Example: `{1, 3, 5, 7, …}` becomes `2[1, ∞)`.

5. **Model Interprocedural Behavior:**
   Add nodes for function calls and returns, propagating parameter and return value information.
   Stack arguments are retrieved from caller memory regions; register arguments from register value sets.
   Upon function exit, registers and stack values are restored.

6. **Address Indirect Jumps/Calls:**
   Where possible, resolve targets via value sets; otherwise, conservatively ignore them.

### Affine Relations

Loops introduce imprecision, as value sets could otherwise grow unboundedly.
Affine relation analysis[^5] improves precision by expressing relationships between variables:

$$
a_0 + \sum_{i=1}^n a_i r_i = 0
$$

This allows inference of bounds for one register based on others.

**Example: Array Initialization Loop**

```asm
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

Here:
`eax = ecx*8 + edx`

Since `ecx < 10`, we deduce:
`eax = 8[0, 9] + &arr[0] = {0, 8, …, 72} + &arr[0]`

This increases precision when modeling memory accesses.

### Call Strings

Call strings[^7] provide **context-sensitive interprocedural analysis** by recording call paths.
Each unique call string corresponds to a separate abstract store, improving precision.
However, this approach is computationally more expensive, as each function must be re-analyzed per distinct call path.

Example of valid call strings:
`{{1,2,4}, {1,2,3}, {1,3,4}}`
This excludes invalid paths such as `1→2→3→4`.

Call strings are typically bounded to prevent explosion in recursive or deep call chains.

## Aggregate Structure Identification (ASI)

The output of VSA includes:

* Value sets for registers, globals, stack, and heap
* Detected indirect memory accesses
* Memory state changes across program points

ASI uses this information to recover **variables, arrays, and structures**.

For stack analysis, size can be inferred from stack-pointer adjustments (e.g., `sub esp, 80` → 80-byte frame).
Memory accesses are used to split the region into smaller **atoms**.

Example:

```asm
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

Here, the 80-byte region is divided into 10 elements of 8 bytes each, suggesting an array.
Structure-like accesses appear as fixed offsets from a pointer:

```asm
mov [eax], 1
mov [eax+4], 2
mov [eax+16], 3
```

ASI results are fed back into VSA, refining abstract locations and improving subsequent iterations.
Iterations continue until results converge, after which information is passed to the **type analysis** phase.

## Improvements

The main drawback of VSA (and affine relation analysis) is **runtime cost**[^2][^6], especially when multiple VSA–ASI iterations are required.
Handling **self-modifying code** also remains challenging.

Improvements include:

* **DVSA (TIE system)**[^4]: Uses SSA-form IR and linear combinations over strided intervals, improving precision and simplifying value-set computation.
* **SecondWrite**[^3]: Suggests runtime optimizations for DIVINE to make it more scalable.
* **Ghidra**[^8]: Performs VSA intraprocedurally and ignores calls, floating point arithmetic and heap regions.

## Code Examples

[Ghidra](https://github.com/NationalSecurityAgency/ghidra/blob/master/Ghidra/Features/Decompiler/src/decompile/cpp/heritage.cc)

[^1]: Cifuentes, Cristina. Reverse compilation techniques. Queensland University of Technology, Brisbane, 1994.
[^2]: Balakrishnan, Gogul, and Thomas Reps. "Divine: Discovering variables in executables." International Workshop on Verification, Model Checking, and Abstract Interpretation. Berlin, Heidelberg: Springer Berlin Heidelberg, 2007.
[^3]: ElWazeer, Khaled et al. "Scalable Variable and Data Type Detection in a Binary Rewriter." PLDI '13: Proceedings of the 34th ACM SIGPLAN Conference on Programming Language Design and Implementation. 2013.
[^4]: Lee, JongHyup, Thanassis Avgerinos, and David Brumley. "TIE: Principled reverse engineering of types in binary programs." (2011)
[^5]: M¨uller-Olm, Markus and Helmut Seidl. "Precise Interprocedural Analysis through Linear Algebra." POPL '04. 2004.
[^6]: Balakrishnan, Gogul, and Thomas Reps. Analyzing memory accesses in x86 executables. Comp Construct. 2004.
[^7]: M. Sharir and A. Pnueli. "Two approaches to interprocedural data flow analysis". In Program Flow Analysis: Theory and Applications, chapter 7, pages 189–234. Prentice-Hall, 1981.