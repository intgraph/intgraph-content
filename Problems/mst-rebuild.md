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

As we described above, the brute force way to solve the problem, that is, find out the new MST in the new graph, has the time complexity $O(ElogV)$. It's not a slow one, but we need something cooler.

Suppose we add a node "X" into the original graph, and now we have a new edge list.

Think about the principle about the Kruskal algorithm. We keep adding the minimal edge into the graph to build the "miminal spanning forest" until we make the forest into a tree. That is, if two node are connected before the current edge is added, this edge will keep unused.

In our scenario, it's not hard to point out that the order of "original edges" will stay the same. So, before an unused edge "<A, B>" is added into the graph, the node "A" and "B" must be connected. So the unused edge in the origial MST will keep unused in the new MST.

So, we can design our algorithm like this:

1. Get the original MST
2. add the edges of the MST and all the new-coming edges to a priority queue
3. use Kruskal algorithm to find out the new MST
