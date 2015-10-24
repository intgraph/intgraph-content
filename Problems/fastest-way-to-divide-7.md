[Metadata]
title: Fastest way to divide 7
date: 2015-10-24 22:40:14

[Tags]
difficulty: 4
categories: bit manipulation
source: mitbbs


[Description]

Please find a **computational fast way** to divide an integer by 7.

> Hint:
> Try to use bit manipulation.

[Solution]

Of course, this problem is not make any sense today. Thanks to the optimization of CPU instruction and compiler, the simple `x = x / 7` will do the trick.

Just come back to the problem. As the bit manipulation is the basic instructions of the CPU which runs much faster than any others, we should only use (or try our best to) the operators `>>` , `<<`, `+` and `-`.

It's easy to point out that `x = x / 7` is similar to `x = x / 8`. And `x = x / 8` can be also represent by `x = x >> 3` with bit manipulation method. But how to calculate `x = x / 7` with `x = x / 8`?

$$
\frac{x}{7} - \frac{x}{8} = \frac{x}{56} = \frac{\frac{x}{8}}{7}
$$

According to the formula above, can you find the right way to solve the problem?

Assume `f(x) = x / 7`, then `f(x) = (x >> 3) + f((x >> 3) + (x & 7))`. The `x & 7` is the reminder for the division. And for a unsigned integer, there are at most 32 bit in one single number. So we will do the same thing for 11 times.

The code can be written in this way:

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <ctime>
#include <cassert>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x


#define DIV7_HELPER(x, y) do { \
    y += (x >> 3); \
    x = (x >> 3) + (x & 7); \
} while(0)

uint32_t divide_7(uint32_t x) {
    uint32_t y = 0;
    x += 1;
    // 11 times
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    DIV7_HELPER(x, y);
    return y;
}

int main() {
    srand(time(nullptr));

    for (int i = 0; i < 1000000; i++) {
        uint32_t x = rand() * rand();
        uint32_t a = divide_7(x);
        uint32_t b = x / 7;

        if (a != b) {
            print(x << ' ' << a << ' ' << b);
            break;
        }
    }
    return 0;
}
```
