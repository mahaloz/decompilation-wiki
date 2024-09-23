# Schema-based Structuring
## Introduction
Schema-based structuring is a type of structuring that depends on a set of known compiler graph patterns[^1].
With this in mind, a decompiler must know all of the compiler graph patterns to generate code that is structured[^3] (contains no gotos).
An example of this type of structuring can be found in the [overview](/fundamentals/structuring/overview) section. 
Schema-based structuring techniques are the most popular techniques among decompilers[^1][^2][^4][^5][^6].

## Example Graph Patterns
Example graph patterns, from Cifuentes 1994 dissertation[^1], can be seen below:

![](/static/img/dcc_schema.png)

## Approaches
After the foundational dissertation from Cifuentes, the Phoenix[^2] work improved on structuring by adding more condition patterns. 
These patterns allowed for more correct structures (like loop reductions). 

Follow-ups to this work all used gotoless[^3][^4] structuring methods until the SAILR[^5] work in 2024.
The SAILR work improved on the gotoless algorithms by introducing a new type of schema that "reverts compiler optimizations."


[^1]: Cifuentes, Cristina. Reverse compilation techniques. Queensland University of Technology, Brisbane, 1994.
[^2]: Brumley, David, et al. "Native x86 decompilation using Semantics-Preserving structural analysis and iterative Control-Flow structuring." 22nd USENIX Security Symposium (USENIX Security 13). 2013.
[^3]: Yakdan, Khaled, et al. "No More Gotos: Decompilation Using Pattern-Independent Control-Flow Structuring and Semantic-Preserving Transformations." NDSS. 2015.
[^4]: Gussoni, Andrea, et al. "A comb for decompiled c code." Proceedings of the 15th ACM Asia Conference on Computer and Communications Security. 2020.
[^5]: Basque, Zion Leonahenahe, et al. "Ahoy sailr! there is no need to dream of c: A compiler-aware structuring algorithm for binary decompilation." 33st USENIX Security Symposium (USENIX Security 24). 2024.
[^6]: Ďurfina, Lukáš, et al. "Design of a retargetable decompiler for a static platform-independent malware analysis." Information Security and Assurance: International Conference, ISA 2011, Brno, Czech Republic, August 15-17, 2011. Proceedings. Springer Berlin Heidelberg, 2011.

