# Type Recovery Overview
## Introduction
Type recovery is the process of identifying high-level variables and their types from a binary[^1], usually in the form of a CFG.
This wiki groups variable identification with type recovery since they are often intertwined in decompilation[^2]. 

In most decompilers, the typing system requires a well-formed CFG, usually lifted, on which to perform analysis. 
Analysis phases can be thought to work in two refining stages:

1. Variable Identification: discover initial locations and their boundaries
2. Type Constraining: utilizing the variables' uses in the code, make constraints for size and choose a type.

This process is often iterative between constraint building and variable identification. 
Variable identification also includes retyping/resizing as more constraints are gathered. 

![](/static/img/typing.svg)

## Typing Example
A simple C program is shown below:
```c
int main(int argc, char** argv) {
    char* str = argv[1];
    puts(str);
}
```

After compiling and disassembling:
```bash
gcc example.c -o example && objdump -D -M intel example | grep "<main>:" -A 12
```

We are left with the following:
```asm
0000000000001149 <main>:
    1149:	f3 0f 1e fa          	endbr64
    114d:	55                   	push   rbp
    114e:	48 89 e5             	mov    rbp,rsp
    1151:	48 83 ec 20          	sub    rsp,0x20
    1155:	89 7d ec             	mov    DWORD PTR [rbp-0x14],edi
    1158:	48 89 75 e0          	mov    QWORD PTR [rbp-0x20],rsi
    115c:	48 8b 45 e0          	mov    rax,QWORD PTR [rbp-0x20]
    1160:	48 8b 40 08          	mov    rax,QWORD PTR [rax+0x8]
    1164:	48 89 45 f8          	mov    QWORD PTR [rbp-0x8],rax
    1168:	48 8b 45 f8          	mov    rax,QWORD PTR [rbp-0x8]
    116c:	48 89 c7             	mov    rdi,rax
    116f:	e8 dc fe ff ff       	call   1050 <puts@plt>
```

A naive variable recovery algorithm might do the following:
```c
int main(int a1, long long a2) {
    int v1; // rbp-0x14
    long long v2; // rbp-0x20
    long long v3; // rax
    long long v4; // rbp-0x8
    v1 = a1;
    v2 = a2;
    v3 = *(&v2 + 1);
    v4 = v3
    puts((char *) v4);
}
```

However, since `puts` is known to take a `char *` as the first argument this would allow `v4` to be constrained to be a `char *`.
Back-propagating this type constraint to the earlier variables, we get the following:
```c
int main(int a1, char** a2) {
    int v1; // rbp-0x14
    char** v2; // rbp-0x20
    char * v3; // rax
    char * v4; // rbp-0x8
    v1 = a1;
    v2 = a2;
    v3 = v2[1];
    v4 = v3
    puts(v4);
}
```

## Variable Identification
Variable identification seeks to map memory locations and registers to high-level variables in the targeted language output (usually C).
Early decompilers often mapped variables to locations based on their simple accesses [^2].
Later work has followed up on this by expanding the set of uses supported for a variable location identification [^1]. 
Identified variables often have many candidates for size (because of their potential type).
These candidates have often been reduced to a single type based on their type sinks[^3], uses that have explicit types like in a call argument. 

## Type Constraining
Type constraining is directly linked to variable identification since the use of the variable is affected by its size (arrays vs primitive types).
Previous works in this area have looked at multiple ways to gather and reduce types for variables.
Some of these include: def-use analysis[^1][^5][^6], library awareness[^3][^5][^6], emulation[^4], lattice-solving[^7][^8], and machine learning[^9][^10].

Of these works, typing involving lattice-based methods[^7][^8], also known as push-down typing, is the most popular among modern open-source decompilers. 


[^1]: Balakrishnan, Gogul, and Thomas Reps. "Divine: Discovering variables in executables." International Workshop on Verification, Model Checking, and Abstract Interpretation. Berlin, Heidelberg: Springer Berlin Heidelberg, 2007.
[^2]: Mycroft, Alan. "Type-based decompilation (or program reconstruction via type reconstruction)." European Symposium on Programming. Berlin, Heidelberg: Springer Berlin Heidelberg, 1999.
[^3]: Lin, Zhiqiang, Xiangyu Zhang, and Dongyan Xu. "Automatic reverse engineering of data structures from binary execution." Proceedings of the 11th Annual Information Security Symposium. 2010.
[^4]: Slowinska, Asia, Traian Stancescu, and Herbert Bos. "Howard: A Dynamic Excavator for Reverse Engineering Data Structures." NDSS. 2011.
[^5]: Haller, Istvan, Asia Slowinska, and Herbert Bos. "Mempick: High-level data structure detection in c/c++ binaries." 2013 20th Working Conference on Reverse Engineering (WCRE). IEEE, 2013.
[^6]: Jin, Wesley, et al. "Recovering C++ objects from binaries using inter-procedural data-flow analysis." Proceedings of ACM SIGPLAN on Program Protection and Reverse Engineering Workshop 2014. 2014.
[^7]: Noonan, Matt, Alexey Loginov, and David Cok. "Polymorphic type inference for machine code." Proceedings of the 37th ACM SIGPLAN Conference on Programming Language Design and Implementation. 2016.
[^8]: Lee, JongHyup, Thanassis Avgerinos, and David Brumley. "TIE: Principled reverse engineering of types in binary programs." (2011)
[^9]: Zhang, Zhuo, et al. "Osprey: Recovery of variable and data structure via probabilistic analysis for stripped binary." 2021 IEEE Symposium on Security and Privacy (SP). IEEE, 2021.
[^10]: Chen, Qibin, et al. "Augmenting decompiler output with learned variable names and types." 31st USENIX Security Symposium (USENIX Security 22). 2022.