[Metadata]
title: Detect the differences between two large files at different servers
date: 2015-03-16 21:47:08 

[Tags]
difficulty: 2
categories: thinking in reverse
source: Aha Insight

[Description]
Poor Mrs. Jones tried to gey past the bubble gum machine before her twins notices. But she failed.

The both twins wanted one gum, and they wanted the gums with same color.

As there's no way of knowing the color of next ball of that machine, how many pennies must Mrs. Jones be prepared to spend in the **worst case**? 

And, what if Mrs. Jones have more children?

p.s. One piece of bubble gum values one penny.

### Input / Output

``n`` stands for the number of colors in the machine. ``k`` stands for the number of child.

And the n numbers in the next line indicate how many gums are with the particular color.

One example:

```
Input:
2 2
4 6
Output:
3
```

One other example:

```
Input:
3 3
6 4 1
Output:
6
```

[Solution]
Let's start with the case of twins.

In the worst case, the first two gums come out of that machine are different, so a third gum (no matter what color it is) is needed.

It's easy for us the generalize the case of gums with ``n`` colors and two children. If there are ``n`` sets of gums, one must be prepared to buy ``n + 1`` balls.

If we have more children, and there are plenty of gums in the machine, the pennies we have to prepared is up to ``k * (n - 1) + 1``. Because the bad luck, before we get ``k`` balls in a same color, we have to buy the gums as much as possible to spend the money.

And the scenario that the gums are less than plenty is easy enough, you can get it easily.

```python
(n, k) = map(int, raw_input().split())
ns = map(int, raw_input().split())
if max(ns) < k:
    print -1
else:
    print sum(map(lambda x: k - 1 if x >= k else x, ns)) + 1
```
