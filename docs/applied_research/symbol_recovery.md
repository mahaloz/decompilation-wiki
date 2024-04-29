# Symbol Recovery 
## Introduction 
A symbol, in the context of binaries, is a name associated with an object.
In most cases, this is either function names or variable names. 
It is often useful for reverse engineering to have the original symbols to more quickly understand the purpose of an object. 

## Symbol Recovery Example
Below is a snippet of a C program:
```c
int mode;
char* name;
long long timezone;
```

After compiling and [stripping](https://en.wikipedia.org/wiki/Strip_(Unix)), a common developer practice, the binary will be decompiled to something like:
```c
int v1;
char* v2;
long long v3;
```

Assuming the types are recovered perfectly (hard), it is still hard to understand what these variables do. 


## Previous Work 
Research in this area has been concerned with the recovery of both variable names[^1][^2][^4][^5][^6][^7] and function names[^3][^5]. 
Approaches have varied between using neural networks[^2][^3][^6][^7], machine translation[^4], probabilistic methods[^5], and BERT-based language models[^1].
In many cases, the bottleneck of this work has been dataset generation[^1].


[^1]: Pal, Kuntal Kumar, et al. ""Len or index or count, anything but v1": Predicting Variable Names in Decompilation Output with Transfer Learning." 2024 IEEE Symposium on Security and Privacy (SP). IEEE Computer Society, 2024.
[^2]: Dramko, Luke, et al. "DIRE and its data: Neural decompiled variable renamings with respect to software class." ACM Transactions on Software Engineering and Methodology 32.2 (2023): 1-34.
[^3]: Artuso, Fiorella, et al. "Function naming in stripped binaries using neural networks." arXiv preprint arXiv:1912.07946 (2019).
[^4]: Jaffe, Alan, et al. "Meaningful variable names for decompiled code: A machine translation approach." Proceedings of the 26th Conference on Program Comprehension. 2018.
[^5]: He, Jingxuan, et al. "Debin: Predicting debug information in stripped binaries." Proceedings of the 2018 ACM SIGSAC Conference on Computer and Communications Security. 2018.
[^6]: DIRE: A Neural Approach to Decompiled Identifier Naming
[^7]: Chen, Qibin, et al. "Augmenting decompiler output with learned variable names and types." 31st USENIX Security Symposium (USENIX Security 22). 2022.