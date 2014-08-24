[Metadata]
title: Paint a map
date: 2014-08-24 16:46:01 

[Tags]
difficulty: 4
categories: proof, constructive
source: Algorithms: A Creative Approach 

[Description]
There's a wierd land, which divided by n distinct lines into different regions.

Your mission is to draw a map that no two neighboring regions share the same color. And two regions are considered neighbors if and only if they have an edge in common.

How many colors will you take to paint a map with n distinct lines.

There are some examples.

![map with 2 lines](http://wizmann-pic.qiniudn.com/37e4e79b47ec6aa00fb911d12d34588d)

![map with 3 lines](http://wizmann-pic.qiniudn.com/c3b50cea0c1bf1245ce1f4a92fd444ca)

[Solution]
As the two examples above, we can make a hypothesis that it will take only two kind of colors to paint a map with arbitrary lines.

Now, let's prove it is correct.

``Map(n) = True`` stands for that a map with n lines could be well painted by only two colors. 

So, ``Map(1) = Map(2) = Map(3) = True``.

Next, assuming a "well painted" map, Map(n), which contains n lines. And we add one line to the map, and divide the map into two parts which are called X & Y.

It's easy to find out that the part X and part Y are "well painted" because the origin map is "well painted". Further, if X (or Y) reverse the color of the regions of the part, they are "well painted", either.

And we considering two regions, R1 and R2, which are in the same region in the origin map, and they are neighbours now. And if we revert the color of X (or Y), as a result, all pairs like <R1, R2> are in different colors.

So, we prove that if we have a ``Map(n) = True``, we can get the new ``Map(n + 1) = True``. As we already get ``Map(1) = True``, so the hypothesis is proved.
