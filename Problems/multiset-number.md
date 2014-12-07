[Metadata]
title: Multiset Number
date: 2014-11-21 09:28:32 

[Tags]
difficulty: 4.5
categories: combination
source: zhihu

[Description]

In mathematics, the notion of multiset (or bag) is a generalization of the notion of a set in which members are allowed to appear multiple times or zero times. The number of each item is infinity.

Now give you N kinds of elements, and how many distinct multiset of set M can be formed?

**The order of the element is irrelevant.**

The input will give the N and M, your mission is to calculate the number of distinct multiset.

```
>> INPUT
10 3

<< OUTPUT
27
```
[Solution]

Try not to treat this problem as an ordinary combination problem. But it may lead you a deadend.

Consider this. The multiset can be present in this way $S = \\{x1 \cdot a1 + x2 \cdot a2 + \cdots\\}$. And it's easy for us to know that $M = \sum(a_1...a_n)$. As a result, the problem can be solve by partition the m numbers into n parts. Since n might greater than m, so there may be some part which is empty.

Seperating m number in to n parts need (n - 1) delimiters. So the final result is like this.

```mathjax
ans =  \mathrm C_m^{n + m - 1}
```

For example, we want to fill a bag(multiset) of size 12 with 8 types of items.

That is, the bag can be represented as, ``1 1 1 1 1 1  1 1 1 1 1 1``, each ``1`` is a empty space for the items.

And then, we put 7 delimiters (why 7?) to the bag.

One of the conditions is like this: ``1 1 * * 1 1 * 1 1 1 * 1 * 1 * * 1 1 1``. The ``1``s before the first star ``*`` indicates the number of the first item. As there is no ``1``s between the first ``*`` and the second ``*``, so there is no item belongs to the second type. 

