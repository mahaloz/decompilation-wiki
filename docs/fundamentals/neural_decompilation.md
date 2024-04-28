# Neural Decompilation
## Introduction 
This wiki defines a neural decompiler as the following:

- _Any decompiler that wholly replaces one or many [fundamental decompiler components](/fundamentals/overview#generic-decompilation-pipeline) with a machine-learning model_.

As such, decompilation produced by a neural decompiler is neural decompilation.
Approaches in this area are promising because they often offer a more generic solution to decompilation. 
Instead of a decompiler developer having to program a component, the component is learned from binaries and their corresponding source[^1]. 

## Previous Works
The earliest academic work in this area is the 2018 work "Using Recurrent Neural Networks for Decompilation"[^1]. 
The work utilized a Recurrent Neural Network (RNN) to replace all components downstream from CFG recovery, omitting the lifting phase to train on x86 assembly. 
Subsequent work also focused on x86, while changing their method for decompilation to Neural Machine Translation[^2][^3][^4][^5][^7] and a more refined RNN[^6].
The most reliable of these works, Coda, claims an average of "82% program recovery accuracy on unseen binary sample(s)"[^6].
The notable Beyond-The-C, also introduces early work to turn binary code into multiple languages other than C [^7].

With the recent popularization of Large Language Models (LLM), like [ChatGPT](https://en.wikipedia.org/wiki/ChatGPT), early work has utilized LLMs for end-to-end decompilation[^8]. 
Other related work in the area has used LLMs to improve the structured output of other decompilers[^9]. 


[^1]: Katz, Deborah S., Jason Ruchti, and Eric Schulte. "Using recurrent neural networks for decompilation." 2018 IEEE 25th International Conference on Software Analysis, Evolution and Reengineering (SANER). IEEE, 2018.
[^2]: Katz, Omer, et al. "Towards neural decompilation." arXiv preprint arXiv:1905.08325 (2019).
[^3]: Liang, Ruigang, et al. "Neutron: an attention-based neural decompiler." Cybersecurity 4 (2021): 1-13.
[^4]: Liang, Ruigang, et al. "Semantics-recovering decompilation through neural machine translation." arXiv preprint arXiv:2112.15491 (2021).
[^5]: Cao, Ying, et al. "Boosting neural networks to decompile optimized binaries." Proceedings of the 38th Annual Computer Security Applications Conference. 2022. 
[^6]: Fu, Cheng, et al. "Coda: An end-to-end neural program decompiler." Advances in Neural Information Processing Systems 32 (2019).
[^7]: Hosseini, Iman, and Brendan Dolan-Gavitt. "Beyond the C: Retargetable decompilation using neural machine translation." arXiv preprint arXiv:2212.08950 (2022).
[^8]: Tan, Hanzhuo, et al. "LLM4Decompile: Decompiling Binary Code with Large Language Models." arXiv preprint arXiv:2403.05286 (2024).
[^9]: Hu, Peiwei, Ruigang Liang, and Kai Chen. "DeGPT: Optimizing Decompiler Output with LLM."