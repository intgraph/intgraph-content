[Metadata]
title: Find the Star
date: 2014-10-30 09:34:15 

[Tags]
difficulty: 3.5
categories: mathmatical induction, graph
source: Algorithms: A Creative Approach

[Description]

A party is being held in a backyard. And among the crowd, there is a star which is well-known that everyone who participates this party knows him. But, as a star, he must keep "高冷", that is, he doesn't know anyone in this party.

Your mission is to find the star from the crowd. Since you don't have any idea about the relationiship among the crowd, you have to make a query like this: "Do you know that crazy person?"

What is the fast way to find out the star? Or determine that there is not a real star among the crowd.

[Solution]

As described above, everyone in the party knows the star, and the star doesn't know anyone. As a result, if A knows B, A can't be the star. And if A doesn't know B, B can't be the star.

So, we can exclude one person from the "star list" by a single query. The time complexity is no doubt O(n).

```python
def find_the_star(n):
    star = 0
    for i in xrange(1, n):
        if do_know(star, i):
            star = i
    for i in xrange(0, n):
        if star != i and \
                do_know(star, i) or \
                not do_know(i, star):
            star = -1
            break
    return star
        
```
