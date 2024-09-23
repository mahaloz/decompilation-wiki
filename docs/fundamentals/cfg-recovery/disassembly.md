# Disassembling
## Introduction

Work in this area dates back to the 90s and can generally be broken up into two disassemblying techniques[^1]:

1. Linear Sweep[^2]: starts disassembling from the first byte in the program and linearly continues
2. Recursive Traversal[^3]: disassembles by following control flow as it is discovered

Linear sweep is considered to be more prone to errors in programs that embed data in the middle of their code[1]. 
Additionally, all disassembling techniques confront the main problem of Instruction boundary identification (IBI)[^4], which dictates when an instruction starts and ends.

## Previous Works
Relevant work for decompilation focused on improving both the accuracy and scalability of disassembling programs[^2][^6]. 
Some of these works have improved these methods for work on deobfuscated binaries[^3] as well as benign ones.
Recent work in the area has focused on reassemblable disassembly[^4][^5].
This area is promising for decompilation as it makes re-compilable decompilation more achievable [^5]. 

There has also been work to measure the accuracy of these disassembling frameworks by comparing their results to that of an instrumented compiler[^7][^8]. 

[^1]: Kruegel, Christopher, et al. "Static disassembly of obfuscated binaries." USENIX security Symposium. Vol. 13. 2004.
[^2]: Free Software Foundation. GNU Binary Utilities, Mar 2002. https://www.gnu.org/software/binutils/manual/. 
[^3]: C. Cifuentes and K. Gough. Decompilation of Binary Programs. Software Practice & Experience, 25(7):811-829, July 1995. 
[^4]: Flores-Montoya, Antonio, and Eric Schulte. "Datalog disassembly." 29th USENIX Security Symposium (USENIX Security 20). 2020.
[^5]: Ruoyu Wang, Yan Shoshitaishvili, Antonio Bianchi, Aravind Machiry, John Grosen, Paul Grosen, Christopher Kruegel, and Giovanni Vigna. Ramblr: Making reassembly great again. In NDSS, 2017
[^6]: Andriesse, Dennis, et al. "An {In-Depth} Analysis of Disassembly on {Full-Scale} x86/x64 Binaries." 25th USENIX security symposium (USENIX security 16). 2016.
[^7]: Pang, Chengbin, et al. "Ground truth for binary disassembly is not easy." 31st USENIX Security Symposium (USENIX Security 22). 2022.
[^8]: Pang, Chengbin, et al. "Sok: All you ever wanted to know about x86/x64 binary disassembly but were afraid to ask." 2021 IEEE symposium on security and privacy (SP). IEEE, 2021.