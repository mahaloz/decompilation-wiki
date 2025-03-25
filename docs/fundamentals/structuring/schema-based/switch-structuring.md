# Switch Structuring
## Introduction
In this article I will focus on the methods outlined in the structuring algorithm for the Phoenix Decompiler[^1]. In the simplest form the switch uses an indirect jump of a calculated distance from the base address of a jump table to the specific case statement. Here is a simple C example:

```C
#include <stdio.h>
#include <stdlib.h>

int main() {
  int x = rand() % 5;
  switch (x) {
    case 0:
      printf("hello\n");
      break;
    case 1:
      printf("hello2\n");
      break;
    case 2:
      printf("hello3\n");
      break;
    case 3:
      printf("hello4\n");
      break;
    case 4:
      printf("hello5\n");
      break;
    default:
      return 0;
  }
  return x;
}
```

and the corresponding disassembly produced by <a href="https://github.com/angr/angr/tree/master">angr</a>:

```assembly
0x401169:       endbr64
0x40116d:       push    rbp
0x40116e:       mov     rbp, rsp
0x401171:       sub     rsp, 0x10
0x401175:       call    0x401070 ; rand()
0x40117a:       movsxd  rdx, eax
0x40117d:       imul    rdx, rdx, 0x66666667
0x401184:       shr     rdx, 0x20
0x401188:       sar     edx, 1
0x40118a:       mov     ecx, eax
0x40118c:       sar     ecx, 0x1f
0x40118f:       sub     edx, ecx
0x401191:       mov     dword ptr [rbp - 4], edx
0x401194:       mov     ecx, dword ptr [rbp - 4]
0x401197:       mov     edx, ecx
0x401199:       shl     edx, 2
0x40119c:       add     edx, ecx
0x40119e:       sub     eax, edx
0x4011a0:       mov     dword ptr [rbp - 4], eax
0x4011a3:       cmp     dword ptr [rbp - 4], 4
0x4011a7:       ja      0x401222
0x4011a9:       mov     eax, dword ptr [rbp - 4]
0x4011ac:       lea     rdx, [rax*4]
0x4011b4:       lea     rax, [rip + 0xe6d]
0x4011bb:       mov     eax, dword ptr [rdx + rax]
0x4011be:       cdqe
0x4011c0:       lea     rdx, [rip + 0xe61]
0x4011c7:       add     rax, rdx
0x4011ca:       notrack jmp     rax
0x401222:       mov     eax, 0
0x401227:       jmp     0x40122c
0x401200:       lea     rax, [rip + 0xe11] ; case 0
0x401207:       mov     rdi, rax
0x40120a:       call    0x401060
0x4011cd:       lea     rax, [rip + 0xe30] ; case 1
0x4011d4:       mov     rdi, rax
0x4011d7:       call    0x401060
0x4011ef:       lea     rax, [rip + 0xe1b] ; case 2
0x4011f6:       mov     rdi, rax
0x4011f9:       call    0x401060
0x401211:       lea     rax, [rip + 0xe07] ; case 3
0x401218:       mov     rdi, rax
0x40121b:       call    0x401060
0x4011de:       lea     rax, [rip + 0xe25] ; case 4
0x4011e5:       mov     rdi, rax
0x4011e8:       call    0x401060
0x40122c:       leave
0x40122d:       ret
0x40120f:       jmp     0x401229
0x4011dc:       jmp     0x401229
0x4011fe:       jmp     0x401229
0x401220:       jmp     0x401229
0x4011ed:       jmp     0x401229
0x401229:       mov     eax, dword ptr [rbp - 4]
```
and here is a simplified control flow graph:

![](/static/img/switch-flow.svg)

## Compiler Representations
### Jump Table
The switch statement above loads the base offset of the jump table from memory, and then computes an indirect JMP to that address offset by the % calculation. The offset is multiplied by 4 to account for the size of each address in memory (rdx = rax * 4). This computation is what allows us constant time when performing a simple switch statement. The indirect branch is particularly important, because it is rare to see a non-hardcoded unconditional JMP instruction, therefore anytime we see such a situation we can mark it as a potential switch.

### Multiple Jump Tables
In the situation where we have large distance between sets of numbers in our switch, the compiler may create multiple jump tables as a simple way to retain constant time in our switch statement. The following C code shows such an example where multiple jump tables would be created:

```C
#include <stdio.h>
#include <stdlib.h>

int main() {
  int x = rand() % 5;
  switch (x) {
  case 0:
    printf("hello\n");
    break;
  case 1:
    printf("hello2\n");
    break;
  case 2:
    printf("hello3\n");
    break;
  case 1340:
    printf("hello4\n");
    break;
  case 1341:
    printf("hello5\n");
    break;
  case 1342:
    printf("hello6\n");
    break;
  // and so on...
  default:
    return 0;
  }
  return x;
}
```

### Balanced Tree
Binary Search Trees (BSTs) and Ternary Search Trees (TSTs) are used by LLVM and GCC respectively[^4] when it is not possible to implement a jump table(s)[^2]. In our first figure we showed a C program containing simple and consecutive case numbers (0, 1, 2, 3, 4) this means it is easy to calculate an offset by just looking up the index associated with it, but if the case constants are not close or a constant distance apart the compiler prefers a ST over large amounts of unused memory. The following C code shows a situation where a ST would be compiled:

```C
#include <stdio.h>
#include <stdlib.h>

int main() {
  int x = rand() % 5;
  switch (x) {
    case 0:
      printf("hello\n");
      break;
    case 10:
      printf("hello2\n");
      break;
    case 200:
      printf("hello3\n");
      break;
    case 301:
      printf("hello4\n");
      break;
    case 457:
      printf("hello5\n");
      break;
    default:
      return 0;
  }
  return x;
}
```

### Cascascaded Conditionals
In a few cases the compiler may deem the best representation of the switch statement to be a set of if, else-if, and else's, in this case the techniques outlined in the phoenix paper[^1] would decompile them back to cascaded conditionals, which can result in a rather messy output from the decompiler. SAILR[^3] introduces some techniques for identifying/reverting this, and producing a more accurate output.

## Structuring
We must now <i>switch</i> up our discussion from the different ways in which a compiler may represent a switch statement, to the ways in which we can structure them. Much of this section has been written from the influence of implementations found in the <a href="https://github.com/uxmal/reko/tree/master">Reko Decompiler</a>.

### Standard Switch Structuring
When structuring we visit each of the nodes in post-order and attempt to match against a set of schemas[^1][^5]. 2 of these schemas outlined in the Phoenix paper[^1] represent the 2 types of switch statement structures we can get: Switch and IncSwitch (Incomplete-Switch). If no such statements are found in a particular traversal we may identify switch candidates (covered in the next section).

For us to structure a switch the following must hold:

1. The switch head is the only entry.
2. There must be one node that is exited to, or all switch cases have no successors.

If these conditions are not met we queue the switch and hope other structuring occurs before we procede with its reduction.

The algorithm for structuring a switch is as follows:

1. Identify the switch.
    <ol type="a">
        <li>Indirect jump to a jump table.</li>
    </ol>

2. Identify the cases.
    <ol type="a">
        <li>Simply find all the successors of the switch head.</li>
        <li>The front end of the decompiler identifies jump tables relevent to the function and adds the correct successors of the graph. The entries that are added to this are done by determining the bounds that are possible to be accessed, this is obvious due to the JA instruction which defines an upper bound and the base address being loaded into the RAX register. Any gaps in the jump table should hold the same address as the JA instruction[^6].</li>
    </ol>

3. Produce a switch statement.
4. Relink graph.

### Non-Standard Switch Structuring
If the switch statement is not represented by a jump table It will likely be structured as cascading if/else statements. In this case we can perform a rewrite to make it more legible after we have completed structuring. To achieve this 2 steps need to be performed:

1. Identify the first node in a cascade.

2. Collect all subsequent nodes in a cascade.

Identifying the first node is done by checking for if statements that meet a set of pattern requirements in their condition. The condition for a valid statement can be a simple ==/!=; a sequence of !='s separated by &&'s or a sequence of =='s separated by &&'s.
We then check consecutive if structures to see if they meet this pattern, and if we can collect a sufficient number of statements we can condense it down into a switch.

This however obviously wouldn't work in the case of STs, which uses > and < comparators. When STs occur few decompilers seem correctly identify this, of those that do there is 2 choices: simplify, but maintain if statements (Hex-Rays) or convert to a switch statement (<a href = "https://github.com/angr/angr/blob/73cff39400821e1826906e1944b332c2fda12f2c/angr/analyses/decompiler/region_simplifiers/switch_cluster_simplifier.py#L510">angr</a>). The addition to support this involves simply continuing to traverse for == or != operators to find the leaf nodes of the STs.

### Switch Candidates
This is a term used in the Phoenix paper[^1] to describe a schema that hasn't been matched, but could be potentially a switch. They occur when a switch has extra edges into or out of it. If there are extra incoming edges to nodes other than the switch head we can simply replace the edge with a jump and for extra outgoing edges we have 2 potential choices:

1. If there is an edge from one of the case nodes to the node that immediately post dominates the switch head, we choose the immediate post dominator as our successor to the switch.
    <ol type="a">
        <li>Immediately post dominating means the closest node n where if we reversed all edges on the graph you would have to go through n to get to the switch head.</li>
    </ol>
2. Else we choose the successor to be the node with the highest number of edges from case nodes, that is not a case node itself.

[^1]: Brumley, David, et al. "Native x86 decompilation using Semantics-Preserving structural analysis and iterative Control-Flow structuring." 22nd USENIX Security Symposium (USENIX Security 13). 2013.
[^2]: Eilam, Eldad. "Reversing: Secrets of Reverse Engineering". Wiley. 2005.
[^3]: Basque, Zion Leonahenahe, et al. "Ahoy sailr! there is no need to dream of c: A compiler-aware structuring algorithm for binary decompilation." 33st USENIX Security Symposium (USENIX Security 24). 2024.
[^4]: Liska, Martin: Switch lowering improvements – slideslive. https://slideslive.com/38902416/switch-lowering-improvements. 2017.
[^5]: Cifuentes, Cristina. Reverse compilation techniques. Queensland University of Technology, Brisbane, 1994.
[^6]: Yang, Zack: How C/C++ Compiler Generate Assembly Code For Switch Statement. https://www.zackyang.blog/article/assembly-code-generation-for-switch-statement. 2023.