[Metadata]
title: Little Dima and Equation
date: 2014-08-21 14:57:00

[Tags]
difficulty: 2.5
categories: implementation
source: codeforces

[Description]
Find all integer solutions x (0 < x < 10^9) of the equation:

![formula][1]

where a, b, c are some predetermined constant values and function s(x) determines the sum of all digits in the decimal representation of number x.

### Input

a, b, c (1 ≤ a ≤ 5; 1 ≤ b ≤ 10000; -10000 ≤ c ≤ 10000).

### Output

Print integer n — the number of the solutions that you've found. 

Next print n integers in the increasing order — the solutions of the given equation.

## Link

http://codeforces.com/contest/460/problem/B

[1]: http://intgraph.qiniudn.com/little-dima-and-equation-formula.png

[Solution]
We can't enumerate the value of x because it has a large scope. But the scope of s(x) is tiny that we can find out ``0 <= s(x) <= 81``.

So we enumerate the value of s(x), and trying to get the value of x' by the formula above. Then, we check if s(x') is equal to s(x). If so, that is one feasible solution. 

Be aware that the solution MUST greater than 0 and less than 1e9. If you miss that point, you will lead to a **Wrong Answer**.

```python
(a, b, c) = map(int, raw_input().split())

ans = []
for i in xrange(81 + 1):
    y = b * (i ** a) + c
    if y > 0 and y < (10 ** 9) and sum(map(int, str(y))) == i:
        ans.append(y)

print len(ans)
print ' '.join(map(str, ans))
```
