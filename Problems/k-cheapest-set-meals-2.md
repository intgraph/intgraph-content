[Metadata]
title: K-Cheapest Set Meals - 2
date: 2015-08-04 00:05:52

[Tags]
difficulty: 3.5
categories: sort, merge sort
source: unknown

[Description]

Welcome to IntGraph restaurant. We are offering set meals for our customers.

Each set meal contains P kinds of dishes, such as "main course", "desserts", "beverage", etc. And the price of the set meal is `sum(prices(dishes))`.

Now given the list of main course and dessert. As we are lack of money, please help us to find K cheapest set meals.

```
Sample Input
3 3       // K, P
5 4 3     // the number of each kind of dishes
3 2 1 5 8 // the prices of catagory_1
2 4 3 1   // the prices of catagory_2
9 6 7     // the prices of catagory_3

Sample Output
3 4 4
```

[Solution]

If we get only two catagories, the method for the problem is same as "K-Cheapest Set Meals - 1". And if we get other catagories, the k-cheapest must be the ones which contains the k-cheapest from the first two catagories.

And we can solve the problem with the code which is based on the one from previous problem.

```python
import heapq

def deal(ns, ms, k):
    n, m = len(ns), len(ms)
    ms.sort()

    class Node:
        def __init__(self, a, b):
            self.a = a
            self.b = b
        def __lt__(self, other):
            return self.sum() < other.sum()
        def sum(self):
            return ns[self.a] + ms[self.b]

    q = [Node(i, 0) for i in xrange(n)]
    heapq.heapify(q)

    ans = []
    for i in xrange(k):
        top = heapq.heappop(q)
        ans.append(top.sum())
        if top.b + 1 < len(ms):
            top.b += 1
            heapq.heappush(q, top)
    return ans

(k, p) = map(int, raw_input().split())
ps = map(int, raw_input().split())
res = [0]
for i in xrange(p):
    ns = map(int, raw_input().split())
    res = deal(res, ns, k)

print ' '.join(map(str, res))
```
