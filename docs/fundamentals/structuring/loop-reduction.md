# Loop Reduction
## Introduction
Loop reduction is a technique performed on CFGs in order to identify the basic blocks that are contained within a loop.
Decompilers inherited this from compilers, where the technique is used to enable optimisations such as loop unrolling.
There are several challenges involved in the identification of loops that make this a very interesting topic.

Surprisingly, there are quite a few different algorithms used in decompilers for the identification of loops:

1. Interval Analysis[^1].
2. Dominator-Based[^2].
3. Havlak-Tarjan[^3].

## Interval Analysis
This method was introduced to decompilation by Cristina Cifuentes[^4]. It served an important step in the ability to identify the order of nested loops. It works in the following way:

1. Identify the set of intervals (maximal single entry subgraphs) in a graph.
    <ol type="a">
        <li>A node is added to an interval if all its immediate predecessors are in the interval.</li>
        <li>A new interval is created if the previous condition does not hold for a node.</li>
    </ol>

2. Collapse the nodes into a new graph.

3. Repeat until you have one node. Once this condition is met you will have a derived sequence of graphs[^5].

    ![](/static/img/interval3.svg)

Now the loops have been collected in their relative nesting. To compute the loops in a graph you first constrain yourself to one interval at a time and ignore external edges. In the case of the interval containing 5, 6 and 7 we ignore the edge 7 -> 4 for it to be dealt with in the interval containing 4 and 5-7.

Each interval head is potentially the head of a loop, and can be queried by determining if there is a back-edge to the interval head. Once a loop head is found a path of nodes within the loop from head to the node where the back-edge is found must be followed. I believe the details for this are conveyed well in Cristina's thesis[^4], therefore they have been omitted from this article.

Overall this technique is good, however it is unable to deal with irreducible graphs properly[^7] an important quality of better algorithms. An irreducible graph is one where it forms a shape of the following figure, our goal with the following algorithms is to deal with them in such a way that they do not inhibit our ability to detect reducible loops.

![](/static/img/irreducible1.svg)

## Dominator-Based Loop Reduction
A node n is dominated by another node d if all paths to n contain d. We therefore call d a dominator. To utilise this definition to find loops we will cover the Sreedhar-Gao-Lee (SGL) algorithm[^2].

The dominators found in a control flow graph can be simply represented in what is called a dominator tree. This is constructed by setting a parent's child nodes to those which it immediately dominates. In the following figure the CFG is represented on the left with its corresponding dominator tree on the right.

![](/static/img/dom1.svg)

In the SGL algorithm they combine both the CFG and the dominator tree into one graph named a DJ graph. The DJ graph is named due to each node having a set of edges for the nodes that it immediately dominates 'D' and for the edges in the flow graph 'J'. We can represent our previous diagram in such a manner using red lines for 'D' edges and black lines for 'J' edges.

![](/static/img/dom2.svg)

Such a representation is rare in practice as it adds complexity, therefore my recommendation is to not be funky and keep the CFG and dominator tree separate.

The following steps outline the process of this algorithm:

1. Perform a depth-first search and identify back-edges.
    <ol type="a">
        <li>A back edge occurs if a node w receives an edge from a node v, where v is considered an ancestor of w.</li>
        <li>An ancestor can be identified if 2 properties hold: the preorder number of w is less than or equal to v, and the preorder number of the last descendent of w is greater than or equal to that of v i.e. there exists a path from w to v.</li>
    </ol>

2. Traverse the graph from the deepest level up to the top. Visit all nodes that have that corresponding level in the CFG.
    <ol type="a">
        <li>Check the predecessors of that node, and if one or more back-edges are dominated by the node then we can classify this node as the head of a reducible loop.</li>
        <li>If however we find a back-edge which the node does not dominate we classify this node as the head of an irreducible loop.</li>
    </ol>

3. In the case of a irreducible loop we mark it and deal with it in step 5.

4. In the case of a reducible loop we start at the node where the back-edge is coming from, adding nodes to the loop body until we reach a node which has already been added to the loop body.
    <ol type="a">
        <li>If a node has already been assigned to a loop/s, it's least nested loop header is assigned to the loop body instead. The Union-Find data structure can be used for this purpose (covered in the next section).</li>
        <li>Once all nodes have been added to the loop, they are collapsed.</li>
    </ol>

5. Before we move to the next level, if we have identified any irreducible loops we take the subgraph containing all the nodes at our current level and greater*. We calculate any strongly connected components and reduce them into one node[^6].
    <ol type="a">
        <li>A strongly connected component is a subgraph where every node is reachable from every other node.</li>
    </ol>

\* The subgraph I'm referring to is the one where we have already collapsed the reducible loops to one node.

## Havlak-Tarjan
In 1997 Havlak presented another way[^3], this was a variation on Tarjan's interval analysis that enabled the collapsing of irreducible loops. Similar to the previous algorithm we will use a Union-Find data structure. 

A Union-Find can be employed on disjoint sets i.e. sets with no elements in common. Union combines two sets and Find gets the set of an element. This can be done by using a tree structure and instantiating each node as its own set.

For our purposes we would use union to add another loop/node to an outer loop. Find would be used to get the outer most loop header. In the following visualisation we present the union operation on the 2 loops (4, (5, 6), 7) from the first figure.

![](/static/img/UF1.svg)

On the left if we call Find(6) it returns 5, on the right calling Find(6) returns 4. This satisfies our need to obtain the outermost loop header of a node. A further optimisation can be employed on this, by changing 6's parent to be 4 just before we return from Find.

The following steps outline the process of this algorithm:

1. Perform a DFS and assign the preorder number of each nodes, and the preorder number of its last descendant. 

2. Separate each nodes incoming edges into two classifications: back edge and other edge. 
    <ol type="a">
        <li>A back edge occurs if a node w receives an edge from a node v, where v is considered an ancestor of w.</li>
        <li>An ancestor can be identified if 2 properties hold: the preorder number of w is less than or equal to v, and the preorder number of the last descendent of w is greater than or equal to that of v i.e. there exists a path from w to v.</li>
    </ol>

3. At the same time initialise each node to be a Union-Find set containing only itself.

4. We then go through the nodes in reverse preorder preforming the following steps:
    <ol type="a">
        <li>Iterate through the node's back-edges, if it links to itself we set it as a self-loop. Otherwise, we add the source of the edge to a list.</li>
        <li>Create a worklist from our list and traverse back up to the loop header adding nodes as we go. If at any point we find a node that is not an ancestor of our loop header, then the loop is considered irreducible.</li>
    </ol>

5. Finally we union all nodes in the loop, setting their header as the loop header.

6. To ensure we detect all the loops we can also insert new nodes in the case of a loop header being shared by a reducible and non-reducible back-edge.

## Conclusion
Both the SGL and Havlak-Tarjan algorithms are excellent choices for a decompiler. It must be noted that the algorithms will produce different results, due to differing definitions on how to nest loops. Further modifications has also been suggested by Ramalingam[^8] to ensure a near linear running time for both algorithms. 

[^1]: Tarjan, Robert. Testing flow graph reducibility. Proceedings of the Fifth Annual ACM Symposium on Theory of Computing. 1973.
[^2]: Sreedhar et al. Identifying loops using DJ graphs. ACM Transactions on Programming Languages and Systems (TOPLAS), Volume 18, Issue 6. November 1996.
[^3]: Havlak, Paul. Nesting of Reducible and Irreducible Loops. ACM Transactions on Programming Languages and Systems, Vol. 19, No. 4, July 1997.
[^4]: Cifuentes, Cristina. Reverse compilation techniques. Queensland University of Technology, Brisbane, 1994.
[^5]: Allen, Frances E. Control flow analysis. Proceedings of a symposium on Compiler optimization. 1970.
[^6]: Tarjan, Robert. Depth-First Search and Linear Graph Algorithms. SIAM Journal on Computing. June 1972.
[^7]: Callahan, Tim. Dataflow & Interval Analysis. 15-745: Optimizing Compilers for Modern Architectures. Carnegie Mellon University, March 2005.
[^8]: Ramalingam, Ganesan. Identifying Loops In Almost Linear Time. ACM Transactions on Programming Languages and Systems. March 1999.