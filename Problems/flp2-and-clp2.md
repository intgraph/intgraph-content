[Metadata]
title: Rounding Up/Down to the Next Power of 2
date: 2014-08-17 10:55:28 

[Tags]
difficulty: 4.5
categories: bit manipulation
source: Hacker's Delight

[Description]
We define two functions that are similar to floor and ceiling, but which are directed roundings to the closest integral power of 2, rather than to the closest integer.

Your mission is to implement these two functions with a **branch-free alogirthm**.

[Solution]
The solution with loop or recursion is easy enough as long as you have some simple knowledge about bit manipulation.

The hardest part in this problem is the **branch-free algorithm**. It's truly tough but useful if you want to optimize some of the crucial part of your problem. (Or just remove the defensive checking code, and it's not a joke).

Let's start from ``uint32_t flp2(uint32_t x)``.

The key to implementing ``flp2`` is to find the leftmost bit of the number ``x``. As we can get the rightmost bit of a number by ``lowbit(x) = x & (-x)``, there's no efficient way to find the leftmost bit like that. However, we can set all the bits which on the right of the leftmost 1-bit to "1", and then get the value of ``x - (x >> 1)`` which is the final answer.

The code is like this.

```cpp
uint32_t flp2(uint32_t x) {
    x = x | (x >> 1);
    x = x | (x >> 2);
    x = x | (x >> 4);
    x = x | (x >> 8);
    x = x | (x >> 16);
    return x - (x >> 1);
}
```
The algorithm is based on right-propagating the leftmost 1-bit. And it's a **branch-free algorithm** which runs fast.

The ``clp2`` function is similar to ``flp2``, but it's more ingenious.

```cpp
uint32_t clp2(uint32_t x) {
    x = x - 1;
    x = x | (x >> 1);
    x = x | (x >> 2);
    x = x | (x >> 4);
    x = x | (x >> 8);
    x = x | (x >> 16);
    return x + 1;
}
```

*FNCKING PERFECT!*





