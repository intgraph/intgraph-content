[Metadata]
title: Negative Weighted Edges in Dijkstra Algorithm
date: 2017-02-19 15:04:38

[Tags]
difficulty: 3
categories: graph, induction
source: unknown

[Description]

It is well known that the Dijkstra algorithm is designed for the "single-source shortest path" problem in a graph with non-negative weighted edges.

The problem is, in which circumstances the negative weighted edges will break the correctness of Dijkstra algorithm?

[Solution]

As we discussed before (about a year ago), the wisdom of prove or disprove the correctness is the induction.

So, first thing here is to figure out the induction hypothesis of Dijkstra algorhtm.

> Induction Hypothesis:    
Given a graph and a vertex v, we know the k vertices that are closest to v and the lengths of the shortest path to them.
(Introduction to Algorithm, Udi Manber)

To make it easier, assume that we have a connected graph with `n` vertices, and the starting point is `p`. At the same time, we have already know the the shortest path from the starting point `p` to any other points in the graph.

Accroding to the induction hypothesis, if we find out how to expand the graph `g` we already have correctly, we can solve the problem. The method to expand the graph in Dijkstra algorithm is to find a vertex which has the shortest distance to any vertex in the graph `g`, make it a new graph with size `n + 1` and we mark it as `g'`.

As a result, we have to focus on the expanding process. If this process break the induction hypothesis, it leads to the incorrectness of the algorithm.

Let's review the expanding process. When adding a new vertex to the graph, we have to make sure that the shortest distance for every vertex in graph `g` remains the same. Otherwise, it break the hypothesis that "we know the shortest path from `p` to any other vertex in `g`".

The conclusion is, if the newly added negative weighted edge doesn't interfere with the correctness of the shortest path in the origin graph, it's still OK. Otherwise, we should choose another algorithm, e.g. Bellman-ford, SPFA, etc.

![](http://wizmann-pic.qiniudn.com/17-2-19/64105144-file_1487487621586_6d5a.png)

![](http://wizmann-pic.qiniudn.com/17-2-19/77276054-file_1487487604753_e855.png)

![](http://wizmann-pic.qiniudn.com/17-2-19/98544718-file_1487487681018_13bb.png)