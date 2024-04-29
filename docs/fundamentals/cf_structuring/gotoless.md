# Gotoless Structuring
## Introduction 
Gotoless structuring is a type of structuring that ignores some compiler patterns to make code that contains no gotos[^1].
According to this work, these gotos are signs of unstructured code which is bad for readability[^1][^2]. 

The most famous of these works, and the founder of the area, is the DREAM decompiler[^1].
The DREAM decompiler used reaching conditions on statements to condense and reduce decompilation. 

The followup to this work, which is a hybrid of gotoless and schema-based methods, is the rev.ng[^2] decompiler. 
They used a method called "Combing", which duplicated nodes in the original graph to get rid of unstructured regions. 

[^1]: Yakdan, Khaled, et al. "No More Gotos: Decompilation Using Pattern-Independent Control-Flow Structuring and Semantic-Preserving Transformations." NDSS. 2015.
[^2]: Gussoni, Andrea, et al. "A comb for decompiled c code." Proceedings of the 15th ACM Asia Conference on Computer and Communications Security. 2020.