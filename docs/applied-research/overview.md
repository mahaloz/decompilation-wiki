# Applied Research Overview
Decompiler research that does not neatly fit into one of the [fundamental](/fundamentals/overview) areas is defined here as applied research.
Research in this area contributes to a specific use-case of decompilation that may not necessarily improve base decompilation.

As an example, most researchers would agree that variable name prediction in stripped binaries is an important research area[^1]. 
However, as it stands, variable name prediction does not improve any fundamental research area (except neural decompilation).
As such, we consider it an applied research area, with that target being human-comprehensible decompilation. 

This section is ever-growing as new research areas are explored in decompilation. 
Currently, the following areas exist:

- [Symbol Recovery](/applied-research/symbol-recovery): recovering the names or high-level symbols that are associated with a function or variable
- [Code Similarity](/applied-research/code-similarity): measuring how similar (for various uses) some binary is to another
- [Vulnerability Discovery](/applied-research/vulnerability-discovery): tuning decompilation to be better used for vulnerability discovery


## Other Research 
Some research areas don't have enough work to define a label for them.
The following works are listed here:

- Byte-exact recompilable decompilation[^4]
- Patchable decompilation[^2]
- Verifiable decompilation[^3]
- Higher abstraction support[^5][^6][^7]


[^1]: Pal, Kuntal Kumar, et al. ""Len or index or count, anything but v1": Predicting Variable Names in Decompilation Output with Transfer Learning." 2024 IEEE Symposium on Security and Privacy (SP). IEEE Computer Society, 2024.
[^2]: Reiter, Pemma, et al. "Automatically mitigating vulnerabilities in x86 binary programs via partially recompilable decompilation." arXiv preprint arXiv:2202.12336 (2022).
[^3]: Verbeek, Freek, Pierre Olivier, and Binoy Ravindran. "Sound C Code Decompilation for a subset of x86-64 Binaries." Software Engineering and Formal Methods: 18th International Conference, SEFM 2020, Amsterdam, The Netherlands, September 14–18, 2020, Proceedings 18. Springer International Publishing, 2020.
[^4]: Schulte, Eric, et al. "Evolving exact decompilation." Workshop on Binary Analysis Research (BAR). 2018.
[^5]: Fokin, Alexander, et al. "SmartDec: approaching C++ decompilation." 2011 18th Working Conference on Reverse Engineering. IEEE, 2011.
[^6]: Wu, Ruoyu, et al. "{DnD}: A {Cross-Architecture} deep neural network decompiler." 31st USENIX Security Symposium (USENIX Security 22). 2022.
[^7]: Liu, Zhibo, et al. "Decompiling x86 deep neural network executables." 32nd USENIX Security Symposium (USENIX Security 23). 2023.