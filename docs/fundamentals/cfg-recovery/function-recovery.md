# Function Identification

Function identification is used to identify the instruction boundaries of a function in the binary.
For decompilation purposes, this serves as crucial information for the flow of code in the program.

There have generally been two approaches:

1. Pattern based (with weights)[^1][^2]
2. Machine-learning based[^3]

Modern tools often utilize pattern-based techniques. 

[^1]: Bao, Tiffany, et al. "{BYTEWEIGHT}: Learning to recognize functions in binary code." 23rd USENIX Security Symposium (USENIX Security 14). 2014.
[^2]: Andriesse, Dennis, Asia Slowinska, and Herbert Bos. "Compiler-agnostic function detection in binaries." 2017 IEEE European symposium on security and privacy (EuroS&P). IEEE, 2017.
[^3]: Shin, Eui Chul Richard, Dawn Song, and Reza Moazzezi. "Recognizing functions in binaries with neural networks." 24th USENIX security symposium (USENIX Security 15). 2015.