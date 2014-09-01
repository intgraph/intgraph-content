[Metadata]
title: Number of Power Pairs
date: 2014-09-01 08:41:01 

[Tags]
difficulty: 3.5
categories: sort, math
source: 待字闺中

[Description]
Given two positive integer array, A and B. To find out the number pairs of ``<a, b>`` that ``power(a, b) > power(b, a)``, which ``a`` is belong to array A and ``b`` is belong to array B.

[Solution]
It's hard to prove it directly that which a and b will satisfie ``power(a, b) > power(b, a)``.

We can make a table to enumerate the answer for some numbers.

<table class="table table-striped-white table-bordered">
<thead>
<tr>
 <th>a</th>
 <th>b</th>
 <th>ans</th>
</tr>
</thead>
<tbody><tr>
 <td>1</td>
 <td>1</td>
 <td>False (1 &gt; 1)</td>
</tr>
<tr>
 <td>1</td>
 <td>2</td>
 <td>False (1 &gt; 2)</td>
</tr>
<tr>
 <td>2</td>
 <td>1</td>
 <td>True (2 &gt; 1)</td>
</tr>
<tr>
 <td>10</td>
 <td>100</td>
 <td>True (10^100 &gt; 100^10)</td>
</tr>
</tbody></table>

(If you have a Python Intepreter during the interview, you can make out that fast and correctly.)

As below, we can't find out a clearly rule to help us to find to problem.

The complexity of the problem is that the ``a`` and ``b`` are in the both side of that inequality. If there is a way to move ``a`` to the left and ``b`` to the right, the problem will be much easier.

![inquality](http://wizmann-pic.qiniudn.com/7dc60886b40becb771d673083e7b33e8)

With the inequality transformation, we can turn the origin problem into this: 

> find out the number of pairs ``<a, b>`` which satisifies ``log(a) / a > log(b) / b``

And the final solution is easy and you can make it right here.

```python
import math
A = map(int, raw_input().split())
B = map(int, raw_input().split())

# O(n * logn) Time complexity here
A = sorted(map(lambda x: log(x) / x, A))
B = sorted(map(lambda x: log(x) / y, B))

# O(n) Time complexity here
ans, p, q = 0, 0, 0
while p < len(A) and q < len(B):
    while p < len(A) and A[p] <= B[q]:
        p += 1
    ans += len(A) - p
    q += 1
print ans
```
