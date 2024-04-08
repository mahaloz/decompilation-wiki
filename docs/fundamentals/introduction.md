# Introduction

Modern decompilation is built on many techniques from both binary analysis and classic compilation algorithms. 
Fundamental research in this area focuses on improving base decompilation across all languages.

Some academic work has defined fundamental decompilation research to include three areas [^1]:

1. [Control Flow Graph Recovery](/docs/fundamentals/cfg_recovery/introduction.md): the extraction of (lifted) directed graphs indicating code execution
2. [Type Recovery](/docs/fundamentals/type_recovery/introduction.md): the typing and discovery of variables in the program
3. [Control Flow Structuring](/docs/fundamentals/cf_structuring/introduction.md): the conversion of a CFG to a linear code-like output

As such, this wiki categorizes any work in those areas as fundamental. 
Additionally, since only a few works have used the same metrics[^1][^2][^3] for quality evaluation, **we consider [decompilation quality evaluation](/docs/fundamentals/evaluation/introduction.md) as a fundamental area**.


[^1]: Basque, Zion Leonahenahe, et al. "Ahoy SAILR! There is no need to DREAM of C: A compiler-aware structuring algorithm for binary decompilation." 33st USENIX Security Symposium (USENIX Security 24). 2024.
[^2]: Yakdan, Khaled, et al. "Helping johnny to analyze malware: A usability-optimized decompiler and malware analysis user study." 2016 IEEE Symposium on Security and Privacy (SP). IEEE, 2016.
[^3]: Brumley, David, et al. "Native x86 decompilation using Semantics-Preserving structural analysis and iterative Control-Flow structuring." 22nd USENIX Security Symposium (USENIX Security 13). 2013.