[Metadata]
title: continuous sequence sum to zero
date: 2014-09-13 15:12:18 

[Tags]
categories: hash
difficulty: 3
source: unknown

[Description]
Given an array A, to find the continuous subsequence which sum to zero.

For example,

```
1 1 2 { -1 0 1 } 2 3
```

[Solution]
The first thing is to find how to get the sum of the subsequence in an effecient way. And it's easy for us to know that ``A[i...j] = A[0...j] - A[0...i-1]``.

And it's not effecient enough for us the find the sum to zero subsequence. We have to use a hash table to find out that if ``A[0...i] = x`` which j that ``A[0...j] == -x``.

The time complexity of the program is O(n), and have to use O(n) extra memory space.

```python
// manual test, only print some of the answers
A = map(int, raw_input().split())
d = dict()

tot = 0
for i, a in enumerate(A):
    tot += a
    if tot in d:
        print d[tot] + 1, i, tot
    d[tot] = i
```
