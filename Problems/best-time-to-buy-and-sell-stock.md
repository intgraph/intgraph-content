[Metadata]
title: Best Time to Buy and Sell Stock
date:  2018-03-17 18:27:42

[Tags]
difficulty: 2
categories: dp
source: Leetcode

[Description]

Say you have an array for which the ith element is the price of a given stock on day i.

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

Design algorithms to find the maximum profit.

1. when you were permitted to complete at most **one** transaction
2. when you were permitted to complete at most **two** transactions
3. when you were permitted to complete as many transactions as you like
4. with no limit number of transactions, you must have a "cool down" period for 1 days between any two transactions
5. with no limit number of transactions, you have to pay a transaction fee for each transaction

Sample

```
> Problem1:

Input: [7, 1, 5, 3, 6, 4]
Output: 5

> Problem2:
Input: [7, 1, 5, 3, 6, 4]
Output: 7

> Problem3:
Input: [7, 1, 5, 3, 6, 4, 1, 2, 10]
Output: 16

> Problem4:
Input: [1, 2, 3, 0, 2]
Output: 3

> Problem5:
Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
Output: 8
```

[Solution]

Those 5 problems above is quite similar because it's all about the transactions to buy/sell stocks. But there's different rules that make them different from each other.

But there's a common solution for all those problems. That is, we deduce all the optimal states for current day from the optimal states for previous day.

For example, in problem 1, we maintain 3 states:

1. the max profit if we doesn't buy any stocks (always 0)
2. the max profit if we've bought one stock
3. the max profit if we finished one transaction (buy then sell)

The code could be like this:

```python
INF = 10 ** 10

class Solution(object):
    def maxProfit(self, prices):
        cur, res = -INF, 0
        for price in prices:
            cur, res = max(cur, -price), max(res, price + cur)
        return res
```

We omit the state 1 deliberately because it's always be 0. And variable `cur` is stand for state 2, variable `res` is stand for state 3. Because we can't buy any stock before the first day, we set state 2 to `-INF`, which means the state is meaningless. And if we don't make any transactions, we can make no profit rather loss money, so the state 3 is set to 0 in the beginning.

For problem2, we have to hold 5 states:

1. the max profit if we doesn't buy any stocks (always 0)
2. the max profit if we've bought one stock
3. the max profit if we finished one transaction (buy then sell)
4. the max profit if we've bought another stock after one transaction
5. the max profit if we finished two transactions (buy then sell)

And the code is quite similar to problem 1:

```python
INF = 10 ** 10

class Solution(object):
    def maxProfit(self, prices):
        a, b, c, d = -INF, -INF, -INF, -INF
        
        for price in prices:
            a, b, c, d = max(a, -price), max(b, price + a), max(c, b - price), max(d, c + price)
        
        return max(0, a, b, c, d)
```

For problem 3, it could be:

1. the max profit if we have the stock in hand
2. the max profit if we have no stock in hand ("no buy" or "buy then sell")

```python
INF = 10 ** 10

class Solution(object):
    def maxProfit(self, prices):
        p, q = 0, -INF
        for price in prices:
            p, q = max(p, price + q), max(q, p - price)
        return p
```

For problem 4,

1. the max profit if we have the stock in hand
2. the max profit if we have no stock in hand ("no buy" or "buy then sell"), and we are not in the "cool down" period
3. the max profit if we have no stock in hand, and we are in the "cool down" period

```python
INF = 10 ** 10

class Solution(object):
    def maxProfit(self, prices):
        a, b, c = 0, -INF, -INF
        for price in prices:
            a, b, c = max(a, c), max(b, a - price), max(c, b + price)
        return max(a, b, c)
```

For problem 5, it's similar to problem 3 but adding a transaction fee:

```python
INF = 10 ** 10

class Solution(object):
    def maxProfit(self, prices, fee):
        a, b = 0, -INF
        for price in prices:
            a, b = max(a, b + price - fee), max(b, a - price)
        return max(a, b)
```