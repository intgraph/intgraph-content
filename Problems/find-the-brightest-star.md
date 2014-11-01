[Metadata]
title: Find the Brightest Star
date: 2014-10-30 09:42:39 

[Tags]
difficulty: 3.5
categories: mathmatical induction, graph
source: mitbbs

[Description]

There is another party held on another backyard. All the participants are stars. During the party, they want to find out who is the most popular star, that is, everyone considers that the brightest star is more famous than himself/herself.

There's a function ``cmp(a, b)`` helps us to compare who is more famous. However, the comparison is not transitive. For example, ``cmp(a, b) == 1`` and ``cmp(b, c) == 1``, there is no guarantee that ``cmp(a, c) == 1``.

Please find a way to solve this problem as fast as possible.

[Solution]

This problem is quite same as "Find the Star". It is because if ``cmp(a, b) < 0`` which means that the star ``a`` considers ``b`` is more popular. As a result, star ``a`` can't be the brightest star in the party.

So, we can exclude one star from the "brightest star candidate list" by one comparison. So the time complexity here is O(N). And we have to check if the the one we choose is the "real brightest star", it also take O(N) time here.

```python
def find_the_brightest_star(n, cmp):
    star = 0
    for i in xrange(1, n):
        if cmp(star, i) < 0:
            star = i
    for i in xrange(n):
        if i != star and cmp(i, star) < 0:
            star = -1
            break
    return star

```
