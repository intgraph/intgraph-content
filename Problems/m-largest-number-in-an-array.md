[Metadata]
title: M-largest Number in an Array
date: 2015-06-18 23:26:48 

[Tags]
difficulty: 3
categories: radix sort
source: Facebook, mitbbs

[Description]

Given an unordered array with `n` numbers. Try to find the largest number `m` that there are at least `m` number in the array which is greater or equal to `m`.

Sample Input   

```
3   
9 1 2 4
```

Sample Output

```
2
```

[Solution]

Before you begin dealing with the problem, come up some talented ideas like binary search or something. I politely ask you to write some test cases first.

For example, 

```
> 1 2 3 4 5   
< Answer: 3

> 1 3 5   
< Answer: 2

> 1 1 1
< Answer: 1
```

It's not hard to write the code.

```python
n = int(raw_input())
ns = map(int, raw_input().split())

ns.sort(reverse = True)
ans = 0
for i, num in enumerate(ns):
    if num >= i + 1:
        ans = i + 1
print ans
```

The time complexity here is `O(n * logn)`.

It's cool. But not cool enough.

From the test case we write, it's not hard to know that the range of the answer is `[0, n]` inclusively.

And we can set all numbers which greater than `n` to `n`, and all numbers which less than `0` to `0`. Then, we can use radix sort which takes only O(n) time to deal with the sorting.

The time complexity is O(n).

```python
n = int(raw_input())
ns = map(int, raw_input().split())

bucket = [0 for i in xrange(n + 1)]

for num in ns:
    if num < 0:
        continue
    elif num > n:
        num = n
    bucket[num] += 1
    
cnt = 0
ans = 0
for i in xrange(n, -1, -1):
    cnt += bucket[i]
    if cnt >= i:
        ans = i
        break
print ans
```
