[Metadata]
title: lost number - 2
date: 2015-09-22 23:49:35

[Tags]
difficulty: 3.5
categories: radix sort, in-place algorithm
source: cc150

[Description]

An array[1..n] contains all integers from 0 to n except for one number which is missing. Your task is to find out the missing one.

Additionally, the array is a read-only one. And you can't access an entire interger from A with a single operation. The only thing you can do is **get the jth bit from ith element**, and this operation takes constant time.

Write code to find the missing number. Do it in O(n) time and O(1) extra memory space.

[Solution]

If we can access the entire numbers from array A. The problem can be solve by radix sort, which takes O(n) time and O(1) extra space.

But in this problem, we can't get entire number form the array. Instead, we can only get one bit from one number at one time.

Assuming there is another array B, which contains all integers from 0 to n (no missing integer). Then we count the rightmost bit of all the nubmers in B. Of course, the number of `0`s is `n / 2 + 1`, and the number of `1`s is `(n + 1) / 2`.

As a result, if the number of `0` and `1` of the right most bit of array A is not match the number in array B. We can easily find out the right most bit of the missing number.

Then we use the same method to find out every bit of the missing number. At last, we can definitely get the right answer.

```python
import random
import copy

def solve_1(ns): # <= this is for test
    n = len(ns)
    for i in xrange(n):
        while ns[i] != i:
            a = ns[i]
            if a >= n:
                break
            if ns[i] == ns[a]:
                break
            ns[i], ns[a] = ns[a], ns[i]

    for i in xrange(n):
        if ns[i] != i:
            return i
    return None

def solve_2(ns): # <= the solution for this problem
    n = len(ns) + 1
    res = 0
    for i in xrange(32):
        ones, zeros = 0, 0
        for j in xrange(n):
            if j & (1 << i):
                ones += 1
            else:
                zeros += 1

        cnt_0, cnt_1 = 0, 0
        for num in ns:
            if num & (1 << i):
                cnt_1 += 1
            else:
                cnt_0 += 1
        if cnt_0 == zeros:
            res |= (1 << i)
    return res

def test():
    n = random.randint(1, 1000)
    ns = range(n)
    random.shuffle(ns)
    ns[0] = n # <= replace the "missing number" with n
    random.shuffle(ns)

    ns1 = copy.deepcopy(ns)
    ns2 = copy.deepcopy(ns)

    a = solve_1(ns1)
    b = solve_2(ns2)

    assert a == b

for i in xrange(100):
    test()
```
