[Metadata]
title: MST Rebuild
date: 2015-09-05 15:44:33

[Tags]
difficulty: 4
categories: MST
source: anonymous


[Description]

MST, a.k.a minimum spanning tree, is a subgraph connects all the vertices in an undirected graph with minimum weight.

![Alt text](http://wizmann-pic.qiniudn.com/e51c3b22414d5492ea38e90fcf63d8a2)

(The picture comes from the wikipedia page: [Minimum Spanning Tree](https://en.wikipedia.org/wiki/Minimum_spanning_tree]))

Now given an connected undirected graph, and we have already find out the MST of the graph. And then, a new vertex add to the graph, together with some edges, forms a new **connected undirected graph**.

The mission is to rebuild the MST with the minimum cost (both space and time).

The simple way to solve the problem is build the MST on the new graph, and we all know that too well. Your algorithm must be more optimal than that.

[Solution]

First, let recall the methods to get a MST. *Prim* and *Kruskal* are two commonly used algorithm for a MST. The time complexity of Prim is $O(V^2)$, or $O(E log(V))$ with a binary heap. And the time complexity of Kruskal is $O(ElogE) = O(ElogV)$ because there is a sort operation in the algorithm.

As we described above, the brute force way to solve the problem, that is, find out the new MST in the new graph, has the time complexity $O(ElogV)$. It's not a slow one, but we need something cool.

![Alt text](http://wizmann-pic.qiniudn.com/13cbeeb3d38a316f8d6c720727e6ef0f)

Suppose we've already got a MST with a root called "A" (of course, any point in the MST could be the root). And we add one new vertex called "X". According to the principle of Prim and Kruskal algorithm, we can't simply add this vertex and the edges connects to it into the current MST.

For a MST, every sub-tree of the MST is a sub-MST. As a result, as we set the new vertex "X" as the root of the MST, "X" connects to some sub-MST. And it's easy to know that every edges of the sub-MST only contains the edges belongs to the original MST.

So, we can design our algorithm like this:

1. Get the original MST
2. add the edges of the MST and all the new-coming edges to a priority queue
3. use Kruskal algorithm to find out the new MST
