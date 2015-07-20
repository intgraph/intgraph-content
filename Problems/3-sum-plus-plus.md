[Metadata]
title: 3-Sum++
date: 2015-07-19 17:35:12

[Tags]
difficulty: 2.5
categories: implementation
source: Facebook

[Description]

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find the number of unique triplets in the array which gives the sum of the given target.

Additionally, each number of the list can be used more than once. That why this problem is called `3sum++`.

[Solution]

This problem is similar to the original `3sum` problem. The code is right here:

```python
def three_sum_plus(nums, target):
    res = 0
    nums.sort()
    n = len(nums)
    for p in xrange(n):
        q = p
        r = n - 1
        while q <= r:
            if nums[p] + nums[q] + nums[r] == target:
                res += 1
                q += 1
                r -= 1
                continue

            if nums[p] + nums[q] + nums[r] > target:
                r -= 1
            elif nums[p] + nums[q] + nums[r] < target:
                q += 1

    return res

if __name__ == '__main__':
    T = int(raw_input())
    for _case in xrange(T):
        target = int(raw_input())
        print three_sum_plus(
                map(int, raw_input().split()),
                target)
```
