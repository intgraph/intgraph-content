[Metadata]
title: Random Sampling
date: 2015-10-18 01:29:55

[Tags]
difficulty: 3
categories: random
source: vintage

[Description]

Now we've got a list of numbers, the mission is to pick k number randomly with equal possibility.

Be ware, we don't know the length of the list. In the other word, we are dealing with a stream of numbers.

[Solution]

The solution for the problem is called [reservoir sampling][1].

We can deal the problem with code like this.

```python
import random

K = 3
N = 16
TEST_TIME = 1000000

nums = range(N)
random.shuffle(nums)
cnt = [0 for i in xrange(N)]


def reservior_sampling(stream, k):
    res = stream[:k]
    for i, item in enumerate(stream[k:]):
        u = i + k + 1
        v = random.randint(0, u - 1)
        if v < k:
            res[v] = item
    return res

for i in xrange(TEST_TIME):
    res = reservior_sampling(nums, K)
    for item in res:
        cnt[item] += 1

for item in cnt:
    print '%.3f' % (1.0 * item / TEST_TIME)
```

And the possibily for each number to be selected is:

![](http://wizmann-pic.qiniudn.com/15-10-18/16200277.jpg)

But here comes the problem: how the code works?

The idea of the solution is come from the mathmatical induction (for me).

Case #1: (trivial)    
we select k numbers from a stream with size k.

It's a trival case that every can solve it by put all the k numbers to the result array.

Case #2:

As we know how to k-random sampling from a stream with n - 1 numbers. We can generalize the method to a stream with n numbers.

For the new comer, we will discard it with possibility `(n - k) / n`. Becase, theoretically, the possibility for the new comer to be chosed is `k / n`.

For the numbers in the result list, the possibility for a number to stay in the result list is:

![](http://wizmann-pic.qiniudn.com/15-10-18/28010464.jpg)

From Case1 & Case2, we can easily find out that the solution above is proved to be correct.

  [1]: https://en.wikipedia.org/wiki/Reservoir_sampling
