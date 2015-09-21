[Metadata]
title: Wiggle Sort
date: 2015-09-22 01:52:25

[Tags]
difficulty: 3
categories: implementation, induction
source: Leetcode


[Description]

Given an arbitary array, and re-arrange it to wiggle style in one pass.

We call an array "wiggle" if the array is ordered in these ways:

```
[1] A0 >= A1 <= A2 >= A3 .... .... An.
[2] A0 <= A1 >= A2 <= A3 .... .... An.
```

[Solution]

The first and foremost thing we need to do to solve this problem is to make the induction hypothesis.

> Induction Hypothesis     
We can wiggle sort an array which length is `n`

It is trivial if there is only one element in the array.

And suppose we've got a wiggle array with length n, what we have to do now is adding a new num to the wiggle array.

For example, the wiggle array is `A[0...n]`, and the new comming number `A[n + 1]` should be less or equal than `A[n]`. So there are two scenarios:

1. `A[n + 1] <= A[n]`     
this case is trivial, we simply append the new element to the back of the original array, then everything is fine
2. `A[n + 1] > A[n]`    
it's easy to find out that if we simply add the new comer to the back, this array is not in the "wiggle style". but as `A[n - 1] <= A[n] <= A[n + 1]`, if we swap `A[n]` and `A[n + 1]`, then we get `A[n - 1] <= A[n + 1] >= A[n]`, it's a "wiggle style" array.

And the case will be the exactly same if we try to find `A[n + 1] >= A[n]`.

As a result, we can easily find out that if we've got a `A[0...n]` wiggle array, it's easy to make a `A[0...n+1]` wiggle array. And it have proved that `A[0]` is a trival wiggle array. So all we have to do is to give an arbitrary rule for our wiggle array (`A[0] <= A[1]` or `A[0] >= A[1]`), then keep pushing new elements to it.

```python
ns = map(int, raw_input().split())
n = len(ns)

for i in xrange(1, n):
    if i % 2 == 0:
        if ns[i] > ns[i - 1]:
            ns[i], ns[i - 1] = ns[i - 1], ns[i]
    else:
        if ns[i] < ns[i - 1]:
            ns[i], ns[i - 1] = ns[i - 1], ns[i]

print ns

```
