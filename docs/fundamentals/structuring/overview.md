# Structuring Overview
## Introduction

Academically introduced in Dr. Cifuentes' 1994 Dissertation[^1], decompilation control flow structuring is the process used to turn a control flow graph (CFG) into a structured high-level language. 
Control flow structuring in decompilation is highly related to the general control flow structuring process found in compiler research.
Although the goal of control flow structuring, referred to as structuring, is to output linear high-level code, the level of abstraction of that code is open research.
Additionally, few works exist in structuring for targeting language output other than C. 

## General Structuring Example
Although often thought of [in the context of assembly](https://en.wikipedia.org/wiki/Decompiler#Structuring), control flow structuring requires a control flow graph and conditions [^3]. 
For example, the attributed control flow graph below can be used as input:

```
                            +-----+
                            |  A  |
                            +-----+
                              |   |
                           ~x |   +--+
                              V      |
                         +-----+     |
                         |  B  |     | x
                         +-----+     |
                           |  +--+   |
                        ~y |     | y |
                           V     V   V
                       +-----+  +-----+
                       |  D  |  |  C  |
                       +-----+  +-----+
                            |    |
                            V    V
                            +-----+
                            |  E  |
                            +-----+
```

Using a [schema-based](/fundamentals/structuring/schema-based) structuring algorithm, the graph can be turned into the following C:

```c
A();
if(x)
    goto label_c;
B();
if (y) {
label_c:
    C();
}
else {
    D();
}
E();
```

There are multiple ways of turning the graph into linear C code [^4].
For instance, the first condition on `x` can be flipped, changing where the `goto` appears and how many `if` scopes exist in the program. 

## Types of Structuring
There are two dominant types of structuring algorithms[^8]:

1. [Schema-based](/fundamentals/structuring/schema-based): Algorithms that construct code based on pre-known graph patterns that are omitted by compilers. These algorithms attempt to only make structured code when they are aware of a direct mapping to its source structure. 
2. [Gotoless](/fundamentals/structuring/gotoless): Algorithms that prioritize removing all unstructured regions from code. These algorithms may use schema-based methods initially but are unique in their pattern-matching of structures that may not exist in its source. 

The biggest difference between these two is their reliance on known compiler patterns[^5]. 
In schema-based algorithms, the decompiler author creates a set of known compiler output patterns to recover a target language. 
In gotoless algorithms, the decompiler author uses patterns outside of graph schemas, which may be compiler and language-independent.
One such example is condition-based structuring, as used in the DREAM [^5] decompiler. 

## Related Fields
Structuring in decompilation was directly inspired by compiler works in structuring and general data-flow analysis[^3]. 
One of these earliest works was the 1970s paper "Control flow analysis."[^2], which laid out the fundamental ideas for constructing [control flow graphs](/fundamentals/cfg-recovery/overview).
Additionally, many of the ideas for eliminating gotos, which were often a byproduct of schema-based structuring, were inspired by work in restructuring source code[^6] [^7].


[^1]: Cifuentes, Cristina. Reverse compilation techniques. Queensland University of Technology, Brisbane, 1994.
[^2]: Allen, Frances E. "Control flow analysis." ACM Sigplan Notices 5.7 (1970): 1-19.
[^3]: Brumley, David, et al. "Native x86 decompilation using Semantics-Preserving structural analysis and iterative Control-Flow structuring." 22nd USENIX Security Symposium (USENIX Security 13). 2013.
[^4]: Basque, Zion Leonahenahe. “30 Years of Decompilation and the Unsolved Structuring Problem: Part 1.” Mahaloz.Re, 2 Jan. 2024, https://mahaloz.re/dec-history-pt1. Accessed 11 Apr. 2024. 
[^5]: Yakdan, Khaled, et al. "No More Gotos: Decompilation Using Pattern-Independent Control-Flow Structuring and Semantic-Preserving Transformations." NDSS. 2015.
[^6]: Williams, M. Howard, and G. Chen. "Restructuring pascal programs containing goto statements." The Computer Journal 28.2 (1985): 134-137.
[^7]: Erosa, Ana M., and Laurie J. Hendren. "Taming control flow: A structured approach to eliminating goto statements." Proceedings of 1994 IEEE International Conference on Computer Languages (ICCL'94). IEEE, 1994.
[^8]: Basque, Zion Leonahenahe, et al. "Ahoy sailr! there is no need to dream of c: A compiler-aware structuring algorithm for binary decompilation." 33st USENIX Security Symposium (USENIX Security 24). 2024.
