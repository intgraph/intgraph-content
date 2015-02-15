[Metadata]
title: Random with 0/1
date: 2015-02-10 17:38:03

[Tags]
difficulty: 3.5
categories: bit manipulation, random
source: yahoo, mitbbs

[Description]

Given a function `rand2()`, which can generate random number 0 and 1 with equal possibility.

Your mission is to implement functions such as `rand4()`, `rand5()` and `rand(N)` with `rand2()`. These functions should generate random from 0 to (N - 1) with equal possibility.

[Solution]

This is a traditional problem of random number. And it's not hard to come up with the way to solve it.

For example, `rand4()`:

```cpp
uint32_t rand4() {
    uint32_t a = rand2();
    uint32_t b = rand2();
    return (a << 1) | b;
}
```

`rand5()` actually is `rand8()`:

```cpp
uint32_t rand5() {
    uint32_t u = 0;
    do {
        u = rand8();
    } while(u >= 5);
    return u;
}

uint32_t rand8() {
    uint32_t a = rand2();
    uint32_t b = rand2();
    uint32_t c = rand2();
    return (a << 2) + (b << 1) + c;
}
```

And, how about `rand(N)`?

First of all, the number N should be rounded to the nearest `$2^t$`. And then, we use a loop the generate a number with is less than N.

```cpp
uint32_t randN(uint32_t n) {
    uint32_t u = clp2(n); // round n to the nearest 2^t
    while (1) {
        uint32_t v = 0;
        for (uint32_t i = 0; (1 << i) <= u; i++) {
            v |= rand2() << i;
        }
        if (v < n) {
            return v;
        }
    }
}
```
    
Now, our problem is function `clp2()`. Of course, we have multiple ways to implement it. But here is the coolest one.

```cpp
uint32_t clp2(uint32_t x) {
    x = x - 1;
    x |= (x >> 1);
    x |= (x >> 2);
    x |= (x >> 4);
    x |= (x >> 8);
    x |= (x >> 16);
    return x + 1;
}
```
