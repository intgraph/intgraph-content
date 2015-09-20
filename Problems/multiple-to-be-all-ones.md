[Metadata]
title: Multiple to be all ones
date: 2014-08-16 15:58:00

[Tags]
difficulty: 3
categories: math
source: itint5, anonymous

[Description]
Given a number **a** (1 <= a <= 10^6), the mission to find the a number **b** that ``a * b`` equals to a number is consists of only ones.

If **b** exists, return the length of a * b. Otherwise, return -1.

For example,

```
a = 3
b = 37
a * b = 111
```

The answer is 3 as len(111) is equal to 3.

[Solution]

Assuming f(n) = 111...1 (n ones).

```
f(n) % a == ((f(n - 1) % a) * 10 + 1) % a
```

So we can enumerate the number of ones to get the final answer.

Further, it's easy to find out that for two numbers, x and y (x != y). For ``f(x) % a == f(y) % a``, we can find that ``f(x + 1) % a == f(y + 1) % a``.

As a result, if we find that pair of numbers (x, y) before we find a number **z** that f(z) % a == 0. It's easy to indicate there is no answer.

```python
class MinAllOne:
    def findMinAllOne(self, a):
        if a == 0:
            return -1
        (v, l) = (1, 1)
        st = set([])
        while True:
            if v % a == 0:
                return l
            else:
                v %= a
                if v in st:
                    return -1
                else:
                    st.add(v)
                v = v * 10 + 1
            l += 1
        return -1
```
