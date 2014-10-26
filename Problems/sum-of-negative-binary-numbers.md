[Metadata]
title: The Sum of Negative Binary Number
date: 2014-10-27 00:26:01 

[Tags]
difficulty: 3.5
categories: simulation, math
source: unknown

[Description]

A negative binary number is a binary number which based on -2. For example, [10] stands for -2 in decimal, [100] stands for 4 in decimal.

The formula of the negative binary number is like this:

```mathjax
a =  \sum_{i=0}^n d_i(-2)^i
```

You are given two decimal numbers (can be positive, zero or negative), your mission is to (1) convert these two numbers into negative binary numbers, and then (2) calculate and print the sum of the neg-binaries.

### Input & Output

The input will contain many cases, each case has two numbers in a line.

The output will contain a negative binary number per line, which is the sum of the two numbers.

```
> Sample Input
-2 -2
-10 12

> Sample Output    
1100        
110
```

[Solution]

First of all, we have to find a way to convert a decimal number to a negtive binary number.

As decribed above, the decimal value of a negtive binary number is a sum of powers of -2. And it is known to us that the decimal value of a oridnary binary number is a sum of powers of 2. Maybe we can make them in the same way.

But here is a problem, the remainder b of ``a % -2 == b`` might be negative as divisor is negative(-2). It's not allowed to put negative digits in the number of course.

Assuming the ith digit of the negative binary number is x, the (i + 1)th digit is y, and ``x < 0``. So, the ith digit stands for ``(-2) ^ i * x``, the (i+1)th digit stands for ``(-2) ^ (i + 1) * y``. And we can make an equaltion here.

```mathjax
\begin{equation}
\begin{split} 
&(-2)^i * x + (-2)^{i+1} * y \\
=& (-2)^i * (x + 2) - (-2)^i * 2 + (-2)^{i+1} * y \\
=& (-2)^i * (x + 2) - (-2)^{i+1} * (y + 1)
\end{split}
\end{equation}
```

By the equation below, we find a way to deal with the negative reminder. It is essential both when we convert a decimal to negative binary number and sum two negative binary numbers.

The second problem is to sum the two neg-numbers. It's not difficult to get the result following the ordinary steps of calculating the sum of two binary numbers.

However, it's not quite same with the oridary one, as the sum of a some digits could be -1, 2 or 3, we have deal with them properly.

The code is right here.

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cassert>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x
#define error(x) cerr << x << endl

string toNeg2(int v) {
    string s;
    while (v != 0) {
        int m = v % 2;
        v = v / -2;
        if (m < 0) {
            m += 2;
            v++;
        }
        s = char(m + '0') + s;
    }
    return s;
}

string neg2Sum(const string& a, const string& b) {
    string c;
    int pa, pb, g;
    pa = a.length() -1;
    pb = b.length() -1;
    g = 0;
    while (pa >= 0 || pb >= 0 || g) {
        int va = pa >= 0? a[pa] - '0': 0;
        int vb = pb >= 0? b[pb] - '0': 0;
        int vc = va + vb + g;
        switch (vc) {
            case -1: {
                g = 1;
                c = "1" + c;
            }; break;
            case 0: case 1: {
                g = 0;
                c = char(vc + '0') + c;
            }; break;
            case 2: case 3: {
                g = -1;
                c = char(vc - 2 + '0') + c;
            }; break;
            default: {
                error("Error!");
                exit(-1);
            }; break;
        }
        pa--; pb--;
    }
    c = c.erase(0, c.find_first_not_of('0'));
    return c;
}

int main() {
    int a, b;
    while (input(a >> b)) {
        string neg_a = toNeg2(a);
        string neg_b = toNeg2(b);

        string neg_c = neg2Sum(neg_a, neg_b);
        int c = a + b;
        if (neg_c != toNeg2(c)) {
            print(a << ' ' << b);
            print(neg_a);
            print(neg_b);
            print(neg_c << ' ' << toNeg2(c));
            assert(neg_c == toNeg2(c));
        }
        print(neg_c);
    }
    error("done!");
    return 0;
}
```
