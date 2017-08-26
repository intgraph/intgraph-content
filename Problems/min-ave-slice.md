[Metadata]
title: Minimal Average Slice
date: 2017-08-26 15:43:30

[Tags]
difficulty: 3
categories: mathmatical induction
source: Codility

[Description]

Given a non-empty, zero-indexed array of integers. Please find out a slice with at least two elements which has the minimal average.

The average of slice `(P, Q)` is the sum of `A[P] + A[P + 1] + ... A[Q]` divided by the length of the slice.

[Solution]

The problem is quite straight forward if we solve it in a brute-force way: just a `n * n` loop to enumerate every possible answer. But, if there is a better way?

As something we always do, we ask mathmatical induction for help. 

Suppose we have already got a slice named `S1` with `l1` elements and the average of this slice is `A1`, and now we have another slice named `S2` with `l2` elements and the average is `A2`. 

Here is the question: under what circumstance `S1 ++ S2` has a less average than `S1`? (`++` means concat)

![](http://wizmann-pic.qiniudn.com/17-8-26/73784555.jpg)

However, if `a2` is less than `a1`, the slice with minimal average should be the slice `S2`.

Here is the conclusion: we should only enumerate all the possible slice with length 2 or 3. Because the slices which are longer than 3 should be split into small pieces which will definitely have at least one sub-slice with less averages.