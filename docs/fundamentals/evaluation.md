# Quality Evaluation
## Introduction 
The best way to measure the quality of overall decompilation is still an open problem in decompilation research.
However, there have been a variety of methods for evaluating the quality of individual [fundamental components](/fundamentals/overview/#generic-decompilation-pipeline) in decompilation.

## Control Flow Structuring Metrics
Early evaluations in control flow structuring utilized metrics from software engineering. 
As such, these metrics were **not** compared to their original source, but instead used standalone. 
This was useful because these metrics could be computed in real-world scenarios of decompilation use and reduced in runtime. 
The following early metrics were used:

- Gotos[^1][^2][^3][^4][^5]
- Full-function Recompilability[^2]
- Lines of Code (LoC)[^4]
- [McCabe Cyclomatic Complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity) (MCC)[^5]

In all but the case of function recompilability, the decompiler would want to optimize for reducing all of these metrics.
However, recent work has argued against the pure reduction of these metrics[^5]. 
The SAILR paper[^5], argues that these metrics should be measured relative to its source (like machine translation).
The following metrics were used as compared to source:

- Gotos
- Boolean Expression
- Function Calls
- [Graph Edit Distance](https://en.wikipedia.org/wiki/Graph_edit_distance) (GED)

In each metric, the score closest to the source is the _best_ structured decompilation. 
Since these metrics can only be used with knowledge of source, they can only be used when developing a decompiler (not optimized in runtime).


## Variable Typing Metrics
Unlike control flow structuring metrics, most evaluations for variable typing have focused on comparing choices to the source[^6][^7][^8][^9].
Generally, these evaluations would flow like the following:

1. Extract the ground-truth types from the source (usually with [DWARF info](https://en.wikipedia.org/wiki/DWARF))
2. Guess variables and types from decompilation
3. See how often (or how close) each chosen variable location and type was to source

Some evaluations have also measured how conservative they were at approximating types[^7].

## Human Evaluation
Some work has utilized human evaluations to understand the quality of decompilation[^10][^11]. 
These works have focused on qualitative metrics, such as users' perception of the code's complexity[^10]. 
Additionally, the Decomperson[^12] paper studied how humans might make decompilation themselves when given assembly.

Since _how_ humans rate decompilation is related to how they reverse engineer, some work has explored how humans reverse and exploit binaries.
These studies have mainly focused on how hackers use tooling and techniques for reverse engineering[^15][^16]. 

## Other General Metrics
Some work has constructed a taxonomy of decompiler-to-source errors[^13].
That work categorized and identified many errors that occur when decompiling a binary. 

Other work has identified general methods by which we may test the _correctness_ of decompilation[^14].
Relatedly, some structuring work has attempted to measure this through recompiliability[^2]. 

## Dataset Generation
The question of "what dataset?" to use has also been an issue in decompilation.
Additionally, compiled binaries can sometimes be hard to access.
Some related work has explored ways to generate bigger binary datasets for the evaluation of binary tools like decompilers[^17].


[^1]: Cifuentes, Cristina. Reverse compilation techniques. Queensland University of Technology, Brisbane, 1994.
[^2]: Brumley, David, et al. "Native x86 decompilation using Semantics-Preserving structural analysis and iterative Control-Flow structuring." 22nd USENIX Security Symposium (USENIX Security 13). 2013.
[^3]: Basque, Zion Leonahenahe, et al. "Ahoy sailr! there is no need to dream of c: A compiler-aware structuring algorithm for binary decompilation." 33st USENIX Security Symposium (USENIX Security 24). 2024.
[^4]: Yakdan, Khaled, et al. "No More Gotos: Decompilation Using Pattern-Independent Control-Flow Structuring and Semantic-Preserving Transformations." NDSS. 2015.
[^5]: Gussoni, Andrea, et al. "A comb for decompiled c code." Proceedings of the 15th ACM Asia Conference on Computer and Communications Security. 2020.
[^6]: Noonan, Matt, Alexey Loginov, and David Cok. "Polymorphic type inference for machine code." Proceedings of the 37th ACM SIGPLAN Conference on Programming Language Design and Implementation. 2016.
[^7]: Lee, JongHyup, Thanassis Avgerinos, and David Brumley. "TIE: Principled reverse engineering of types in binary programs." (2011)
[^8]: Zhang, Zhuo, et al. "Osprey: Recovery of variable and data structure via probabilistic analysis for stripped binary." 2021 IEEE Symposium on Security and Privacy (SP). IEEE, 2021.
[^9]: Chen, Qibin, et al. "Augmenting decompiler output with learned variable names and types." 31st USENIX Security Symposium (USENIX Security 22). 2022.
[^10]: Yakdan, Khaled, et al. "Helping johnny to analyze malware: A usability-optimized decompiler and malware analysis user study." 2016 IEEE Symposium on Security and Privacy (SP). IEEE, 2016.
[^11]: Enders, Steffen, et al. "dewolf: Improving Decompilation by leveraging User Surveys." arXiv preprint arXiv:2205.06719 (2022).
[^12]: Decomperson: How Humans Decompile and What We Can Learn From It
[^13]: Dramko, Luke, et al. "A Taxonomy of C Decompiler Fidelity Issues." 33st USENIX Security Symposium (USENIX Security 24). 2024.
[^14]: How far we have come: Testing decompilation correctness of C decompilers
[^15]: Nosco, Timothy, et al. "The industrial age of hacking." 29th USENIX Security Symposium (USENIX Security 20). 2020.
[^16]: Mantovani, Alessandro, et al. "{RE-Mind}: a First Look Inside the Mind of a Reverse Engineer." 31st USENIX Security Symposium (USENIX Security 22). 2022.
[^17]: Singhal, Vidush, et al. "Cornucopia: A Framework for Feedback Guided Generation of Binaries." Proceedings of the 37th IEEE/ACM International Conference on Automated Software Engineering. 2022.