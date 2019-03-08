[Metadata]
title: Tower Breakers, Revisited!
date: 2019-03-08 22:43:00

[Tags]
difficulty: 3.5
categories: game theroy, SG function
source: Hackerrank

[Description]

Two players are playing a game. In the game, there are N towers. 

For each turn, the player will try to reduce the height for the tower T1 to T2. 

* T1 > T2
* T1 % T2 == 0

The player who can't reduce the height of the towers is the loser of this game.

> Source: [Hackerrank - Tower Breakers, Revisited!][1]

[Solution]

A typical SG function problem.

For each tower, the SG(T) is the number of factors of T minus 1.

For example, 

* SG(2) = mex(1, 2) - 1 = 1
* SG(3) = mex(1, 3) - 1 = 1
* SG(4) = mex(1, 2, 4) - 1 = 2

Then we can xor all the SG(T) of the towers and get the final result.


```python
#!/bin/python

from __future__ import print_function

import os
import sys

#
# Complete the towerBreakers function below.
#

def countFactors(n):
    if n == 1:
        return 0
    i = 2
    cnt = 0
    while i * i <= n:
        while n % i == 0:
            cnt += 1
            n /= i
        i += 1
    if n != 1:
        cnt += 1
    return cnt

assert countFactors(2) == 1

def towerBreakers(arr):
    arr = map(countFactors, arr)
    return 1 if reduce(lambda x, y: x ^ y, arr) else 2


if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    t = int(raw_input())

    for t_itr in xrange(t):
        arr_count = int(raw_input())

        arr = map(int, raw_input().rstrip().split())

        result = towerBreakers(arr)

        fptr.write(str(result) + '\n')

    fptr.close()
```

[1]: https://www.hackerrank.com/challenges/tower-breakers-revisited-1/problem
