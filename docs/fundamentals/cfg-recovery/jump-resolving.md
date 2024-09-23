# Jump Resolving

After recovering a CFG from a binary, there may be calls that have an unknown target. 
Indirect jump resolving is the process of discovering some, or all, of the targets those jumps may go to. 
A basic implementation of this is Switch statements resolving, since they are often compiled into indirect jumps [^1].

The work in this area has mostly focused on ways to reduce the total set of analyzed pointers while doing pointer analysis[^2][^3].
Additionally, these works have used program knowledge to further reduce constant-reducible pointers, like those found in class initialization in C++[^2]. 

## Indirect Jump Example
A simple indirect jump looks like the following in x86:
```asm
jmp [rax];
```

Since pointer analysis is considered hard, it may not be trivial to find the value of `rax`.


[^1]: Brumley, David, et al. "Native x86 decompilation using {Semantics-Preserving} structural analysis and iterative {Control-Flow} structuring." 22nd USENIX Security Symposium (USENIX Security 13). 2013.
[^2]: Kim, Sun Hyoung, et al. "Refining Indirect Call Targets at the Binary Level." NDSS. 2021.
[^3]: Kim, Sun Hyoung, et al. "Binpointer: towards precise, sound, and scalable binary-level pointer analysis." Proceedings of the 31st ACM SIGPLAN International Conference on Compiler Construction. 2022.