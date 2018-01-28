[Metadata]
title: Array and queries - 1
date: 2018-01-28 18:33:14

[Tags]
difficulty: 2.5
categories: array, mathmatical induction
source: HackerRank

[Description]
Given an array, you are asked to find a way to divide this array into subsets, and all the subsets are "beautiful subsets".

A subset `S` of the array is called *"beautiful"* if an only if:

* `len(S) == 1`
* `S` contains a continuous integer sequence from `min(S)` to `min(S) + len(S) - 1`

For example, set `[1]`, `[1, 2, 3]`, `[1000, 1001, 1002]` are all beautiful. But `[]`, `[1, 3, 5]`, `[1, 11, 111]` are appearently not beautiful.

Here we want to know the minimal possible number of subsets that all the subsets are "beautiful".

### Restriction

The length of the array could be as large as `3 * 10^5`.

The element of the array would be in the range of `[1, 10^9]`.

### Sample Input/Output

```
Sample Input
3
1 2 3
5
1 2 3 2 3

Sample Output
1
2
```

### Source

[HackerRank Hiring Contest > Array and Queries](https://www.hackerrank.com/contests/hackerrank-hiring-contest/challenges/array-and-queries-1)

[Solution]

> The first step in solving a problem is recognizing there is one ... **induction hypothesis**.

There is some common patterns for this kind of problem from the first glance. For this problem, it's something like:

> If I've got the optimal solution for array[0...n], then I can make the magic tricks to build the optimal solution for array[0...n + 1]. 

But when we dive deeper, we find it a little bit hard for the "magic tricks" because the given array is in arbitary order. It's hard to prove the correctness of the "magic tricks" and harder to implement them.

So we are trying to strengthen the hypothesis. As the order of the origin array doesn't really matter, we can sort the array right before we start. And the correctness proof will be intuitive and much easier.

Here is the solution:

```python
from collections import defaultdict
def solve(ns):
    d = defaultdict(int)
    for num in ns:
        if d[num - 1] > 0:
            d[num - 1] -= 1
        d[num] += 1
    return sum(d.values())

if __name__ == '__main__':
    T = int(raw_input())
    for case_ in xrange(T):
        ns = sorted(map(int, raw_input().split()))
        print solve(ns)
```