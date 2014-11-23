[Metadata]
title: The Minimal Difference Between Two Sorted Arrays
date: 2014-11-24 00:19:47 

[Tags]
difficulty: 2.5
categories: array
source: mitbbs, google

[Description]

Given two **sorted** arrays, please find the minimal difference between the elements of the two arrays.

That is, $ans = min(a, b) \\quad (a \\in A, b \\in B) $

[Solution]

Assuming A[0] < B[0], and we may have these conditions below.

Cond.1

```
A: 1   8
B:   2
```

Cond.2

```
A: 1   8
B:   7
```

Cond.3

```
A: 1 2
B:     3
```

For all these conditions, we all have to compare `<A[0], B[0]>` and `<A[1], B[0]>`. That is, we `A[i] < B[j]`, the next pair we have to compare is `<A[i + 1], B[j]>`.

And then, we can write code like this.

```python
A = map(int, raw_input().split())
B = map(int, raw_input().split())

la, lb = len(A), len(B)
p, q = 0, 0

ans = 0xdeadbeef

while (p < la and q < lb):
    ans = min(ans, abs(A[p] - B[q]))
    if A[p] < B[q]:
        p += 1
    else: 
        q += 1

print ans
```

If you think it's hard to enumerate all the conditions, there is another way to solve this problem.

Merge sort is one of the well-known algorithm, and this problem is quite similar to this algorithm. We merge the two array into one, and find the minimal difference between the element from the different arrays. That will be excellent, but will cost an extra memory space. But we can change a little and make it work.

```python
A = map(int, raw_input().split())
B = map(int, raw_input().split())

la, lb = len(A), len(B)
p, q = 0, 0
ans = 0xdeadbeef
prev, tp = -1, -1

while p < la and q < lb:
    if A[p] < B[q]:
        now = A[p]
        t = 0
        p += 1
    else:
        now = B[q]
        t = 1
        q += 1

    if tp != -1 and tp ^ t:
        ans = min(ans, now - prev)
    prev = now
    tp = t

print ans
```

A little complicated, but easier to understand. :)
