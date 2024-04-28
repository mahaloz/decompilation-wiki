# Overview
## Introduction

Modern decompilation is built on many techniques from both binary analysis and classic compilation algorithms. 
Fundamental research in this area focuses on improving base decompilation across all languages.

Some academic work has defined fundamental decompilation research to include three areas[^1], this wiki includes an extra fourth one:

1. [Control Flow Graph Recovery](/fundamentals/cfg_recovery/overview): the extraction of (lifted) directed graphs indicating code execution
2. [Type Recovery](/fundamentals/type_recovery/overview): the typing and discovery of variables in the program
3. [Control Flow Structuring](/fundamentals/cf_structuring/overview): the conversion of a CFG to a linear code-like output
4. [Quality Evaluation](/fundamentals/evaluation): the measurement of overall decompilation quality

Each of these fundamental areas affects the quality of one another in some way.
For instance, improvements to type recovery can directly improve the results of control flow structuring[^4].
With this in mind, we included the fourth area, quality evaluation in decompilation, as fundamental.
We include this area because it influences the methodologies for the previous three areas and few works have standardized on metrics[^1][^2][^3].

Other works, such as [function name recovery](/applied_research/symbol_recovery) can be found in the [Applied Research](/applied_research/overview) section.


## Generic Decompilation Pipeline

Modern decompilers are comprised of fundamental components that can be directly mapped to each fundamental research area.
Most decompilers follow a pipeline flow that resembles the following[^3]:

![](/static/img/dec-pipeline.svg)

The evaluation component is optional but has been used in the past for on-the-fly decision-making in other components like control flow structuring[^5].
Each component can be implemented at various levels, such as the optional [lifting](/fundamentals/cfg_recovery/lifting) phase in control flow recovery. 

In the case of [neural decompilation](/fundamentals/neural_decompilation), most components are replaced by the machine learning model. 



[^1]: Basque, Zion Leonahenahe, et al. "Ahoy SAILR! There is no need to DREAM of C: A compiler-aware structuring algorithm for binary decompilation." 33st USENIX Security Symposium (USENIX Security 24). 2024.
[^2]: Yakdan, Khaled, et al. "Helping johnny to analyze malware: A usability-optimized decompiler and malware analysis user study." 2016 IEEE Symposium on Security and Privacy (SP). IEEE, 2016.
[^3]: Brumley, David, et al. "Native x86 decompilation using Semantics-Preserving structural analysis and iterative Control-Flow structuring." 22nd USENIX Security Symposium (USENIX Security 13). 2013.
[^4]: Lee, JongHyup, Thanassis Avgerinos, and David Brumley. "TIE: Principled reverse engineering of types in binary programs." (2011).
[^5]: Gussoni, Andrea, et al. "A comb for decompiled c code." Proceedings of the 15th ACM Asia Conference on Computer and Communications Security. 2020.
