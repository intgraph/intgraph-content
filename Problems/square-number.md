[Metadata]
title: Square Number
date: 2015-04-13 09:28:47  

[Tags]
difficulty: 3
categories: math
source: openjudge

[Description]

Given a positive integer `b`, find the maximum integer `a` to make `a * (a + b)` is a square number. (1 <= b <= 10^9)

```
>> Sample Input
3
1
3
6
<< Sample Output
0
1
2
```

[Solution]

Assuming `a * (a + b) == c ^ 2`, and we may try hard to find something useful from this equation. But it only leads to a deadend.

As we know, `a * (a + b) == a^2 + a * b == c ^ 2`. It's not hard to find out that `a < c`. This gives us clue to build another equation.

![Alt text](http://wizmann-pic.qiniudn.com/7395d5eeb5a4cf5d6db240b6a902682f)

And the number `a` is an integer, and should be as large as possible. As a result, `b - 2t` should be as small as possible, at the meantime, `t` shoule be the maximum.

If `b` is odd number, `b - 2t = 1` will be the best solution to make a largest `a`, `t` is equal to `static_cast<int>(b / 2)`.

If `b` is even, `b - 2t = 2` may be the best solution if `t^2 % 2 == 0`. Or `b - 2t == 4` if the previous solution is not available.

You can test your code [here][1].

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <cmath>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x 

long long solve(int x) {
    int t = 0;
    if (x & 1) {
        t = x / 2;
    } else {
        t = x / 2 - 1;
        if (t & 1) {
            t = x / 2 - 2;
        }
    }
    return static_cast<long long>(t) * t / (x - 2 * t);
}

int main() {
    int T, x;
    input(T);
    while (T--) {
        input(x);
        print(solve(x));
    }
    return 0;
}
```

[1]: http://poj.openjudge.cn/practice/1046/
