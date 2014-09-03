[Metadata]
title: Divide and multiply a number
date: 2014-09-03 08:48:00

[Tags]
difficulty: 3.5
categories: math
source: 待字闺中

[Description]
Given a positive integer ``k``. Your misssion is to divide that number into several positive intgers ``[a0, a1 ... an]`` that ``a0 + a1 + ... + an == k`` to make ``a0 * a1 * ... * an`` as large as possible.

[Solution]
Assuming we have already get the solutions of ``a0, a1... am``, it's easy to know that if ``an == ax + ay``, the result of ``an`` is equal to ``max(solution(ax) * solution(ay))``. This method will take O(n^2) time and O(n) memory space.

```python
n = int(raw_input())
s = [1]
for i in xrange(1, n + 1):
    s.append(i)
    for j in xrange(i/2 + 1):
        s[i] = max(s[i], s[i - j] * s[j])
print s[n]
```

And there's a simple formula for this problem.

```
maxi = 2 ** x + 3 ** y
```
with ``2 * x + 3 * y == n`` and ``y`` should be as large as possible.

Here I give a simple (maybe not right) proof.

The inequality ``(n - x) * x > x`` is true when ``x >= 2`` and ``x <= n - 2``. And it shows that if a number can be divided, then do divide it.

Finally, we get a number of ``2s`` and ``3s``. And it's easy for us that ``2 * 2 * 2 = 8 < 9 = 3 * 3``, so we must turn ``2s`` into ``3s`` as much as possible to make the maximum final result.
