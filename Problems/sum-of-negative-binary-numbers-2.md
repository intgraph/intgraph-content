[Metadata]
title: The Sum of Negative Binary Number - 2
date: 2015-06-20 11:38:11 

[Tags]
difficulty: 3.5
categories: simulation, math
source: codeforces

[Description]

Given number w and m, the mission is to find out if there is a way to represent m as this way.

$$
m = \sum_i^nk_i*w^i \quad (k_i\in[-1, 0, 1], w \gt 1)
$$

The range of m is $[2, 10^9]$ inclusively, and the range of w is $[2, 10^9]$ inclusively.

If there is a way to represent m, print `YES`, if not, print `NO`.

Sample Input/Output

```
> Input
> 3 7
< Output
< YES
--

> Input
> 100 99
< Output
< YES
--

> Input
> 100 50
< Output
< NO
```

Source: [Codeforces Round #308 (Div. 2) - C. Vanya and Scales][1]

[1]: http://codeforces.com/contest/552/problem/C

[Solution]

It's the same problem as [here][1] if `w == 2`, so I called this problem "The Sum of Negative Binary Number - 2", but it's not the binary number actually.

It's fine to share the same points of the two problems. The first step is convert m to w-based number.

```python
def convert(a, b):
    res = []
    while b:
        res.append(b % a)
        b /= a
    return res + [0]
```

For each digit of the w-based number, it OK if the digit is in [-1, 0, 1], because it's in the range of $k_i$. And it's OK too if the digit is in [w - 1, w], because we can borrow the value of the next digit.

So, with the help of some examples, we can make the it work easily. But if you think it's difficult to make some examples, just deal with it with some kinds of brute force algorithm. Not hard, and won't be mentioned here.

```python
import sys

def convert(a, b):
    res = []
    while b:
        res.append(b % a)
        b /= a
    return res + [0]

for line in sys.stdin:
    (a, b) = map(int, line.split())
    c = convert(a, b)
    g = 0
    flag = True
    for num in c:
        num = num + g
        if num in [-1, 0, 1]:
            g = 0
        elif num in [a - 1, a]:
            g = 1
        else:
            flag = False
            break

    if flag:
        print 'YES'
    else:
        print 'NO'
```


[1]: http://intgraph.wizmann.tk/Problems/sum-of-negative-binary-numbers.html

