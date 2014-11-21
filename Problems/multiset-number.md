[Metadata]
title: Multiset Number
date: 2014-11-21 09:28:32 

[Tags]
difficulty: 4.5
categories: combination
source: zhihu

[Description]

In mathematics, the notion of multiset (or bag) is a generalization of the notion of a set in which members are allowed to appear multiple times or zero times.

Now give you N kinds of elements, and how many distinct multiset can be formed? **The order of the element is irrelevant.**

```
>> INPUT
10 3
<< OUTPUT
27
```
[Solution]

Try no to treat this problem as an ordinary combination problem. But it may lead you a deadend.

Consider this. The multiset can be present in this way $S = \\{x1 \cdot a1 + x2 \cdot a2 + \cdots\\}$. And it's easy for us to know that $m = \sum(a_1...a_n)$. As a result, the problem can be solve by partition the m numbers into n parts. Since n might greater than m, so there may be some part which is empty.

Seperating m number in to n parts need (n - 1) delimiters. So the final result is like this.

```mathjax
ans =  \mathrm C_m^{n + m - 1}
```
