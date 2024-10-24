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

## Intermediate Languages

| Name                                                                 | Project          | Notes                                                                                                                                   |
|----------------------------------------------------------------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| [BIL](https://github.com/BinaryAnalysisPlatform/bap)                 | BAP              | Includes another IL called Core Theory                                                                                                  |
| [BNIL](http://docs.binary.ninja/dev/bnil-llil.html)                  | Binary Ninja     | Includes 7 different ILs internally such as Low Level IL, Medium Level, SSA forms, and others                                            |
| [Boogie](https://www.microsoft.com/en-us/research/project/boogie-an-intermediate-verification-language/) | Boogie           | [GitHub Repository](https://github.com/boogie-org/boogie)                                                                               |
| [Cas](https://github.com/bdcht/amoco/blob/release/amoco/cas/expressions.py) | Amoco            |                                                                                                                                         |
| [Chunk IR](https://github.com/columbia/egalito/tree/master/src/chunk) | Egalito          |                                                                                                                                         |
| [DBA](https://link.springer.com/chapter/10.1007%2F978-3-662-46681-0_17) | BINSEC          | [BINSEC](http://binsec.gforge.inria.fr/), [GitHub Repository](https://github.com/binsec/binsec)                                          |
| [ESIL](https://github.com/radare/radare2/wiki/ESIL)                  | Radare           | [Documentation](https://github.com/radare/radare2/blob/master/doc/esil.md)                                                              |
| [Falcon IL](https://github.com/falconre/falcon)                      | Falcon           |                                                                                                                                         |
| [FalkerIL](https://gamozolabs.github.io)                              | Falker*          | Not well-documented; created because existing ILs didn't meet specific purposes                                                          |
| [GDSL](https://github.com/gdslang/gdsl-toolkit)                      | GDSL             |                                                                                                                                         |
| [GRIRB](https://grammatech.github.io/gtirb/)                         | GRIRB            | [GitHub Repository](https://github.com/GrammaTech/gtirb)                                                                                |
| [JEB IR](https://www.pnfsoftware.com/blog/jeb-native-pipeline-intermediate-representation/) | JEB |                                                                                                                     |
| [LowUIR](https://github.com/B2R2-org/B2R2)                           | B2R2             |                                                                                                                                         |
| [Miasm IR](https://github.com/cea-sec/miasm)                         | Miasm            | [Blog Post](https://miasm.re/blog/2019/01/16/miasm_ir_getting_higher.html)                                                              |
| [Microcode](https://hex-rays.com/products/ida/support/ppt/recon2018.ppt) | Hex-Rays        | [Hex Blog](http://www.hexblog.com/?p=1232); not the same as Insight Microcode!                                                          |
| [Microcode](https://github.com/hotelzululima/insight)                | Insight          | [Website](https://insight.labri.fr/); not the same as Hex-Rays Microcode!                                                               |
| [P-Code](http://ghidra.re/courses/languages/html/pcoderef.html)      | GHIDRA           | [Sleigh Documentation](http://ghidra.re/courses/languages/html/sleigh.html#idm140310875627168)                                          |
| [RDIL](https://github.com/REDasmOrg/REDasm)                          | RedASM           |                                                                                                                                         |
| [REIL](https://www.zynamics.com/binnavi/manual/html/reil_language.htm) | BinNavi         |                                                                                                                                         |
| [RREIL](https://bitbucket.org/mihaila/bindead/wiki/Introduction%20to%20RREIL) | Bindead        |                                                                                                                                         |
| [Snowman IR](https://derevenets.com/)                                | Snowman          | [GitHub Repository](https://github.com/yegord/snowman/blob/master/src/nc/arch/x86/X86InstructionAnalyzer.cpp)                            |
| [SSL](http://www.jakstab.org/)                                       | Jakstab          |                                                                                                                                         |
| [TSL](http://pages.cs.wisc.edu/~reps/past-research.html#TSL_overview) | CodeSonar       |                                                                                                                                         |
| [Unnamed](https://github.com/EiNSTeiN-/decompiler/tree/master/src/ir) | EiNSTeiN-        |                                                                                                                                         |
| [VEX](https://github.com/smparkes/valgrind-vex/blob/master/pub/libvex_ir.h) | Valgrind      | [Angr Docs](https://docs.angr.io/advanced-topics/ir), [FOSDEM Slides](https://archive.fosdem.org/2017/schedule/event/valgrind_vex_future/attachments/slides/1842/export/events/attachments/valgrind_vex_future/slides/1842/valgrind_vex_future.pdf) |
| [Vine](http://bitblaze.cs.berkeley.edu/vine.html)                    | BitBlaze         | Based on VEX                                                                                                                           |
| [VTIL](https://github.com/can1357/VTIL/)                             | VTIL             | Designed for binary translation; initially used for defeating VMProtect                                                                  |


## LLVM IR-based Lifting

One of the most popular and widely known IRs for program analysis is LLVM IR [^9], which is used in LLVM-based projects like the [Clang](https://clang.llvm.org/) compiler.
With this in mind, many decompiler/binary analysis platforms have attempted to adopt LLVM IR as their main IR for analysis in the past. 
However, it is still unknown how effective these approaches are since they have not achieved wide-scale adoption in either industry or academia. 

| Name                                                                 | Project                                                             | Notes                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------------|-----------------------------------------|
| [allin](http://sdasgup3.web.engr.illinois.edu/Document/allin_poster.pdf) | allin                                                              |                                         |
| [bin2llvm](https://github.com/cojocar/bin2llvm)                      | S2E                                                                 |                                         |
| [Dagger](https://github.com/repzret/dagger)                          | Dagger                                                              |                                         |
| [fcd](https://github.com/zneak/fcd)                                  | fcd                                                                 |                                         |
| [Fracture](https://github.com/draperlaboratory/fracture)             | Fracture                                                            |                                         |
| [libbeauty](https://github.com/jcdutton/reference)                   | libbeauty                                                           | Has been removed from GitHub, reference URL linked to instead |
| [mctoll](https://github.com/microsoft/llvm-mctoll)                   | mctoll                                                              |                                         |
| [Rellume](https://github.com/aengelke/binopt)                        | binopt                                                              |                                         |
| [remill](https://github.com/trailofbits/mcsema)                      | McSema                                                              |                                         |
| [reopt](https://github.com/GaloisInc/reopt)                          | reopt                                                               |                                         |
| [RetDec](https://github.com/avast/retdec)                            | RetDec                                                              |                                         |
| [revng](https://github.com/revng/revng)                              | revng                                                               | First lifted to [QEMU TCG](https://www.qemu.org/docs/master/devel/index-tcg.html), then LLVM IR |


[^1]: Song, Dawn, et al. "BitBlaze: A new approach to computer security via binary analysis." Information Systems Security: 4th International Conference, ICISS 2008, Hyderabad, India, December 16-20, 2008. Proceedings 4. Springer Berlin Heidelberg, 2008.
[^2]: Kinder, Johannes, and Helmut Veith. "Jakstab: a static analysis platform for binaries: tool paper." Computer Aided Verification: 20th International Conference, CAV 2008 Princeton, NJ, USA, July 7-14, 2008 Proceedings 20. Springer Berlin Heidelberg, 2008.
[^3]: Brumley, David, et al. "BAP: A binary analysis platform." Computer Aided Verification: 23rd International Conference, CAV 2011, Snowbird, UT, USA, July 14-20, 2011. Proceedings 23. Springer Berlin Heidelberg, 2011.
[^4]: Wang, Fish, and Yan Shoshitaishvili. "Angr-the next generation of binary analysis." 2017 IEEE Cybersecurity Development (SecDev). IEEE, 2017.
[^5]: Gussoni, Andrea, et al. "A comb for decompiled c code." Proceedings of the 15th ACM Asia Conference on Computer and Communications Security. 2020.
[^6]: Revng. “Revng/Revng: Revng: The Core Repository of the Rev.Ng Project.” GitHub, github.com/revng/revng. Accessed 27 Apr. 2024.  
[^7]: Van Emmerik, Michael James. Static single assignment for decompilation. University of Queensland, 2007.
[^8]: Engel, Daniel, Freek Verbeek, and Binoy Ravindran. "BIRD: A Binary Intermediate Representation for Formally Verified Decompilation of X86-64 Binaries." International Conference on Tests and Proofs. Cham: Springer Nature Switzerland, 2023.
[^9]: https://llvm.org/docs/LangRef.html
