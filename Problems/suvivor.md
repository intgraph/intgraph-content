[Metadata]
title: Survivor
date: 2015-09-04 00:26:11

[Tags]
difficulty: 3.5
categories: sort, bit manipulation
source: unknown

[Description]

There are 100 people stand in a line, numbered from 1 to k. Each time the people who stand in the even position will be killed until there are only two people survive. The problem is that given the number k, please find out the number of the survior except the first man in the line.

Here is an example, if k = 6:

```
 1 2 3 4 5 6
 |   /   /
 1   3   5
 |       /
(1)      5
```

The answer is 5.

[Solution]

We can make some examples when k = 5, 6, 7, 8. It's easy to find out that the answers are all 5.

From these examples, we can find the rule about how the position changes on every step of killing.

```
pos(x) = (x + 1) / 2
```

For example, the position of the No.5 guy is 5 initially. And then the position changes to 3, then to 2.

Here we get a solution for this problem with `O(n)` time complexity and memory complexity.

```python
def survivor(k):
    dp = [0 for i in xrange(k + 1)]
    trans = lambda x: (x + 1) / 2

    maxi, ans = 0, -1
    for i in xrange(3, k + 1, 2):
        u = trans(i)
        if dp[u] + 1 > maxi:
            maxi = dp[u] + 1
            ans = i
        dp[i] = dp[u] + 1
    return ans

print survivor(100)
```

But if the number K is large, the `O(n)` solution is not acceptable. We need to find a way with better performance.

Considering the first guy in the queue, can you tell the reason why he is always the survivor?

```
First Step: 1
f(1) = (1 + 1) / 2 = 1
Second Step: 1
f(f(1)) = (1 + 1) / 2 = 1
...
...
```

The example above shows that the number `1` will become `10` (in binary), then turn to `1` again after the division. We can make a conclusion that the step that a man could stay alive is decided by the second `1` bit from right in his number represent in binary.

For example, the No.5 guy could stay alive for two steps:

```
First Step: 5 (101)
f(5) = (5 + 1) / 2 = 3 (11)
Second Step: 3 (11)
f(f(5)) = f(3) = 2
Thrid Step: 2 (10) -> Killed
```

It's easy to see that the rightmost `1` bit moves to left, until there is another `1`, then the guy is killed.

We can write code like this:

```python
lowbit = lambda x: x & (-x)

k = int(raw_input())
while k != lowbit(k):
        k -= lowbit(k)
print k + 1
```
