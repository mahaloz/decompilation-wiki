# Vulnerability Discovery
In many uses of decompilation, humans, or machines, aim to understand if a program is safe.
To verify if this program is safe, they attempt to do the opposite: find vulnerabilities in the program.
Some decompilers, and their associated research, have attempted to tune their decompilers to be better at this task[^1].
There has also been work at evaluating decompilers by how well they perform with source tools[^2]. 

Most research in this area has focused on static analysis[^1][^2][^3] and [symbolic execution](https://en.wikipedia.org/wiki/Symbolic_execution)[^4] applied to decompilation.
Since these tasks have often been researched with source, an application to binaries has been achieved through decompilation. 


[^1]: Botacin, Marcus, et al. "Revenge is a dish served cold: Debug-oriented malware decompilation and reassembly." Proceedings of the 3rd Reversing and Offensive-oriented Trends Symposium. 2019.
[^2]: Mantovani, Alessandro, et al. "The Convergence of Source Code and Binary Vulnerability Discovery--A Case Study." Proceedings of the 2022 ACM on Asia Conference on Computer and Communications Security. 2022.
[^3]: Park, Jihee, et al. "Static Analysis of JNI Programs via Binary Decompilation." IEEE Transactions on Software Engineering (2023).
[^4]: Han, HyungSeok, et al. "QueryX: Symbolic Query on Decompiled Code for Finding Bugs in COTS Binaries." 2023 IEEE Symposium on Security and Privacy (SP). IEEE, 2023.