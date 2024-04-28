# Overview
## Introduction
A control flow graph (CFG) is a graph that describes the "flow" of execution in a program[^1]. 
As such, CFG recovery, in binary analysis, is the recovery of such a graph from binary code. 
Since decompilation directly relies on this CFG, the recovery of it is considered fundamental to decompilation. 

This recovery can be broken up into multiple phases, some being optional depending on the decompilation target:

1. [Disassembling](/fundamentals/cfg_recovery/disassembly): the conversion of binary code to its mnemonic instructions and operands 
2. [Program Lifting](/fundamentals/cfg_recovery/lifting): the conversion disassembly to an intermediate language (IL) for better abstraction 
3. [Function Recognition](/fundamentals/cfg_recovery/func_recov): the discovery of boundaries defining a function
4. [Indirect Jump Resolving](/fundamentals/cfg_recovery/jump_res): the resolution of jumps that have no constant target(s). 

The second phase, program lifting, is only required if the decompiler aims to be architecture agnostic.
Most decompilers will use an IL to make their later analyses more widely applicable. 

Methods for evaluating improvements in this field are also of note but have had very limited work.
The most recent of these works has focused on instrumenting and comparing to the compile-generated CFG[^2][^3]. 

## Graph Recovery Example
An example C program is shown below:
```c
int main(int argc) {
    int ret_code;
    if(argc > 2) {
        ret_code = 0;
    }
    else {
	    ret_code = 1;
    }
    return ret_code;
}
```

It is compiled on a x86-64 Linux machine with [GCC](), without optimizations:
```bash
gcc example.c -o example && objdump -D -M intel example | grep "<main>:" -A 12
```

After disassembling with [objdump](https://en.wikipedia.org/wiki/Objdump), which includes some function identification, we get the following disassembly:
```asm
0000000000001129 <main>:
    1129:	f3 0f 1e fa          	endbr64
    112d:	55                   	push   rbp
    112e:	48 89 e5             	mov    rbp,rsp
    1131:	89 7d ec             	mov    DWORD PTR [rbp-0x14],edi
    1134:	83 7d ec 02          	cmp    DWORD PTR [rbp-0x14],0x2
    1138:	7e 09                	jle    1143 <main+0x1a>
    113a:	c7 45 fc 00 00 00 00 	mov    DWORD PTR [rbp-0x4],0x0
    1141:	eb 07                	jmp    114a <main+0x21>
    1143:	c7 45 fc 01 00 00 00 	mov    DWORD PTR [rbp-0x4],0x1
    114a:	8b 45 fc             	mov    eax,DWORD PTR [rbp-0x4]
    114d:	5d                   	pop    rbp
    114e:	c3                   	ret
```

In this example, the instructions at `0x1138` and `0x114a` cause the control flow to branch, resulting in the recovered graph:

![](/static/img/disass_ex.svg)

Notice the implied edges coming from the assembly that looked linear (`0x114a`).
The structure of the graph will also look the same if lifted to an IL.


[^1]: Allen, Frances E. "Control flow analysis." ACM Sigplan Notices 5.7 (1970): 1-19.
[^2]: Pang, Chengbin, et al. "Ground truth for binary disassembly is not easy." 31st USENIX Security Symposium (USENIX Security 22). 2022.
[^3]: Pang, Chengbin, et al. "Sok: All you ever wanted to know about x86/x64 binary disassembly but were afraid to ask." 2021 IEEE symposium on security and privacy (SP). IEEE, 2021.