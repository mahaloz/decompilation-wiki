# Neural Decompilation
## Introduction 
This wiki defines a neural decompiler as the following:

- _Any decompiler that wholly replaces one or many [fundamental decompiler components](/docs/fundamentals/overview#generic-decompilation-pipeline) with a machine-learning model_.

As such, decompilation produced by a neural decompiler is neural decompilation.
Approaches in this area are promising because they often offer a more generic solution to decompilation. 
Instead of a decompiler developer having to program a component, the component is learned from binaries and their corresponding source[^1]. 

## Previous Works
The earliest academic work in this area is the 2018 work "Using Recurrent Neural Networks for Decompilation"[^1]. 
The work utilized a Recurrent Neural Network (RNN) to replace all components upstream from CFG recovery, however, their approach was specific to x86 assembly.  


[^1]: Katz, Deborah S., Jason Ruchti, and Eric Schulte. "Using recurrent neural networks for decompilation." 2018 IEEE 25th International Conference on Software Analysis, Evolution and Reengineering (SANER). IEEE, 2018.

