# Program Lifting 

## Introduction
In program lifting, disassembly is converted into an intermediate language (IL)[^1], which is also referred to as an intermediate representation (IR).
Converting disassembly to an IL allows decompiler developers to make optimizations on the IL level which apply to multiple architectures. 

There exist many ILs for analysis of programs, but some of the most notable ones for decompilation are tied to binary analysis.
Most binary analysis platforms that have created or used an IL often follow the same techniques but mostly differ in their later use of ILs[^1][^2][^3][^4]. 
One such use is recompilable decompilation, which can be made easier by lifting to compiled ILs like LLVM-IR[^5][^6].
Another use-case has been the verification of decompilation correctness[^8].

Similar to static analysis, most ILs used in decompilation support some form of static single assignment (SSA) since it simplifies some analyses[^7].

## Example Lifted Program
Below is some example x86 assembly of a simple C program:
```asm
0000000000001129 <main>:
    1129:   f3 0f 1e fa             endbr64
    112d:   55                      push   rbp
    112e:   48 89 e5                mov    rbp,rsp
    1131:   89 7d ec                mov    DWORD PTR [rbp-0x14],edi
    1134:   83 7d ec 02             cmp    DWORD PTR [rbp-0x14],0x2
    1138:   7e 09                   jle    1143 <main+0x1a>
    113a:   c7 45 fc 00 00 00 00    mov    DWORD PTR [rbp-0x4],0x0
    1141:   eb 07                   jmp    114a <main+0x21>
    1143:   c7 45 fc 01 00 00 00    mov    DWORD PTR [rbp-0x4],0x1
    114a:   8b 45 fc                mov    eax,DWORD PTR [rbp-0x4]
    114d:   5d                      pop    rbp
    114e:   c3                      ret 
```

It can be lifted to an IL like VEX, the IL used in the angr decompiler:
```asm
00 | ------ IMark(0x401129, 4, 0) ------
01 | PUT(rip) = 0x000000000040112d
02 | ------ IMark(0x40112d, 1, 0) ------
03 | t0 = GET:I64(rbp)
04 | t8 = GET:I64(rsp)
05 | t7 = Sub64(t8,0x0000000000000008)
06 | PUT(rsp) = t7
07 | STle(t7) = t0
08 | ------ IMark(0x40112e, 3, 0) ------
09 | PUT(rbp) = t7
10 | PUT(rip) = 0x0000000000401131
11 | ------ IMark(0x401131, 3, 0) ------
12 | t10 = Add64(t7,0xffffffffffffffec)
13 | t13 = GET:I64(rdi)
14 | t25 = 64to32(t13)
15 | t12 = t25
16 | STle(t10) = t12
17 | PUT(rip) = 0x0000000000401134
18 | ------ IMark(0x401134, 4, 0) ------
19 | t14 = Add64(t7,0xffffffffffffffec)
20 | t5 = LDle:I32(t14)
21 | PUT(cc_op) = 0x0000000000000007
22 | t26 = 32Uto64(t5)
23 | t16 = t26
24 | PUT(cc_dep1) = t16
25 | PUT(cc_dep2) = 0x0000000000000002
26 | PUT(rip) = 0x0000000000401138
27 | ------ IMark(0x401138, 2, 0) ------
28 | t29 = 64to32(t16)
29 | t30 = 64to32(0x0000000000000002)
30 | t28 = CmpLE32S(t29,t30)
31 | t27 = 1Uto64(t28)
32 | t23 = t27
33 | t31 = 64to1(t23)
34 | t18 = t31
35 | if (t18) { PUT(rip) = 0x401143; Ijk_Boring }
NEXT: PUT(rip) = 0x000000000040113a; Ijk_Boring
...
```

In the case of VEX, each `t` variable is only ever assigned once, making this SSA form. 
You will also notice how verbose every instruction has become. 
Even simple `mov` instructions, which assign a value to a register, have much more information now. 
The lifted VEX above has been cut for brevity. 

[^1]: Song, Dawn, et al. "BitBlaze: A new approach to computer security via binary analysis." Information Systems Security: 4th International Conference, ICISS 2008, Hyderabad, India, December 16-20, 2008. Proceedings 4. Springer Berlin Heidelberg, 2008.
[^2]: Kinder, Johannes, and Helmut Veith. "Jakstab: a static analysis platform for binaries: tool paper." Computer Aided Verification: 20th International Conference, CAV 2008 Princeton, NJ, USA, July 7-14, 2008 Proceedings 20. Springer Berlin Heidelberg, 2008.
[^3]: Brumley, David, et al. "BAP: A binary analysis platform." Computer Aided Verification: 23rd International Conference, CAV 2011, Snowbird, UT, USA, July 14-20, 2011. Proceedings 23. Springer Berlin Heidelberg, 2011.
[^4]: Wang, Fish, and Yan Shoshitaishvili. "Angr-the next generation of binary analysis." 2017 IEEE Cybersecurity Development (SecDev). IEEE, 2017.
[^5]: Gussoni, Andrea, et al. "A comb for decompiled c code." Proceedings of the 15th ACM Asia Conference on Computer and Communications Security. 2020.
[^6]: Revng. “Revng/Revng: Revng: The Core Repository of the Rev.Ng Project.” GitHub, github.com/revng/revng. Accessed 27 Apr. 2024.  
[^7]: Van Emmerik, Michael James. Static single assignment for decompilation. University of Queensland, 2007.
[^8]: Engel, Daniel, Freek Verbeek, and Binoy Ravindran. "BIRD: A Binary Intermediate Representation for Formally Verified Decompilation of X86-64 Binaries." International Conference on Tests and Proofs. Cham: Springer Nature Switzerland, 2023.