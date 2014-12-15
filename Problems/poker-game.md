[Metadata]
title: Poker Game
date: 2014-12-16 01:34:17 

[Tags]
difficulty: 3
categories: possibility, expectation, dp, induction
source: unknown


[Description]

There are n types of poker card, and the account of each type is infinity. The rule to pick up the card is simple enough, that is, pick one card in one time.

Your misssion is to find out the expectation of the number of cards, which you can get a collection of all n types of cards.

![Card Game](http://wizmann-pic.qiniudn.com/f8be57eed67c01bab3cc61a632e53429)

[Solution]

We use ``F(n, k)`` to represent the expectation of get a collection of k types of all n types cards. And it's easy for us to find out that ``F(n, 1)`` is always equal to 1. It is because you will get one type when you picking up any cards in the pile.

Assuming we have already get the answer of ``F(n, k)``. If we need a new type, the possibility to get it is ``(n - k) / n``. As a result, the expectation to get a new type when we already have got k types is ``n / (n - k)``.

```mathjax
f(n, k + 1) = f(n, k) + \frac{n}{n - k}
```

The code is right here.

```python
p = 1.0
for i in xrange(2, n + 1):
    p = p + 1.0 * n / (n - i - 1)
print p
```

The problem seems difficult, but actually it's quite a easy one. So, please don't be scared by the apparence of the problem, just keep calm, and trying hard to find something **inside**.



