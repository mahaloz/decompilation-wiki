# Code Similarity
## Introduction
In cases such as malware identification, the ability to estimate code similarity among binaries is critical[^1].
Research in this area generally looks at ways to improve the reliability of similarity detection among binaries. 

There is little work in the direct use of decompilation for code similarity, however, the general work in the binary analysis is frequent. 
These works are included here since they often touch on or improve fundamental components in decompilation. 

The most direct research in this area has utilized Ghidra decompilation to identify inlined functions in decompilation[^2]. 

## Related Works
Many works have progressed towards binary-based code similarity that do not explicitly use decompilation [^1][^3][^4][^5][^6].
Most of these works have improved code similarity techniques indirectly by improving it for their specific uses cases. 
These uses have included malware identification[^1], duplicated bug hunting[^3][^4], and code reuse[^5].

Recent work has suggested that machine learning has made significant strides in this area[^6].


[^1]: Hu, Xin, Tzi-cker Chiueh, and Kang G. Shin. "Large-scale malware indexing using function-call graphs." Proceedings of the 16th ACM conference on Computer and communications security. 2009.
[^2]: Ahmed, Toufique, Premkumar Devanbu, and Anand Ashok Sawant. "Finding Inlined Functions in Optimized Binaries." arXiv preprint arXiv:2103.05221 (2021).
[^3]: Feng, Qian, et al. "Scalable graph-based bug search for firmware images." Proceedings of the 2016 ACM SIGSAC conference on computer and communications security. 2016.
[^4]: Eschweiler, Sebastian, Khaled Yakdan, and Elmar Gerhards-Padilla. "Discovre: Efficient cross-architecture identification of bugs in binary code." Ndss. Vol. 52. 2016.
[^5]: Mirzaei, Omid, et al. "Scrutinizer: Detecting code reuse in malware via decompilation and machine learning." Detection of Intrusions and Malware, and Vulnerability Assessment: 18th International Conference, DIMVA 2021, Virtual Event, July 14–16, 2021, Proceedings 18. Springer International Publishing, 2021.
[^6]: Marcelli, Andrea, et al. "How machine learning is solving the binary function similarity problem." 31st USENIX Security Symposium (USENIX Security 22). 2022.

