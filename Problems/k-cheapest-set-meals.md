[Metadata]
title: K-Cheapest Set Meals - 1
date: 2015-08-03 23:30:24

[Tags]
difficulty: 2.5
categories: sort, merge sort
source: unknown

[Description]

Welcome to IntGraph restaurant. We are offering set meals for our customers.

Each set meal contains one main course and one dessert. And the price of the set meal is `Price(main course) + Price(dessert)`.

Now given the list of main course and dessert. As we are lack of money, please help us to find K cheapest set meals.

```
Sample Input
3 5 4     // K, the number of main courses, the number of dessert
3 2 1 5 8 // the prices of main courses
2 3 3 4   // the prices of desserts

Sample Output
3 4 4
```

[Solution]

The simplest way is to enumerate the composition of main courses and desserts. The time complexity of this method is `O(n * m * log(n * m))`.

```python
(k, n, m) = map(int, raw_input().split())
ns = map(int, raw_input().split())
ms = map(int, raw_input().split())
print ' '.join(
    map(str,
        sorted([n_ + m_ for n_ in ns for m_ in ms])[:k]
    )
)
```

This method would not satisfies anybody. Because if we use priority queue, the time complexity will be optimized to `O(log(k) * n * m)`.

However, there is always something better. Here we convert this problem to a "merge sort" like one. We consider that there are N sorted list, each one is formed by `map(lambda x: ns[i] + x, ms)` with the list `ms` sorted.

With this method, we can deal with the problem with time complexity `O(m * log(m) + k * log(n))`.

```
import heapq

(k, n, m) = map(int, raw_input().split())
ns = map(int, raw_input().split())
ms = map(int, raw_input().split())
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
print ' '.join(map(str, ans))

```
