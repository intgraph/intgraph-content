[Metadata]
title: To xor or not to xor
date: 2015-04-30 23:29:13 

[Tags]
categories: xor, matrix, Gaussian Elimination
difficulty: 4
source: sgu

[Description]

The sequence of non-negative integers A1, A2, ..., AN is given. You are to find a sebset of the sequence, such that the xor of the subset has a maximum value.

**The length of the sequence N could be up to 100.**

Sample test(s)

```
Input
3       <- The length of the sequence
11 9 5  <- The numbers in the sequence

Output
14      <- The max xor value for a subset of the sequcence
```

> Source: [Sgu 275](http://acm.sgu.ru/problem.php?contest=0&problem=275)

[Solution]

Yep, it's a hard problem. As the xor is quite a weird operator as we can't use some ways like: "find a largest xor subset of the sequence, and add another number to it" of "seperate the sequence into two, and merge them to find the largest xor sum".

However, xor has one tricky feature. For example, the subset sum of sequence `a, b, c, d` can't be `(a + b + c) + (c + d + a)`, it's not a legal one. But for the subset xor, `(a ^ b ^ c) ^ (c ^ d ^ a)` is a legal one because it is equal to `b ^ d`.

As we want to find the largest subset xor, it's not a bad choice to find a subset which the xor has an "1" at the highest bit. And it's not bad either to find a subset which has a "0" at the highest bit and an "1" at the second highest bit. And ...

That is! We will get some subset of numbers to get more "1" bit as possible, at the same time, the higher bits have more prior.

You may find out that is quite similar to the **Gaussian elimination**. Yes, this is the very key for the problem.

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

const int SIZE = 64;
const int N = 100;

int n, m;
int mat[N][SIZE];

void xor_line(int a, int b) {
    for (int i = 0; i < SIZE; i++) {
        mat[a][i] ^= mat[b][i];
    }
}

void swap_line(int a, int b) {
    for (int i = 0; i < SIZE; i++) {
        swap(mat[a][i], mat[b][i]);
    }
}

void do_gauss(int l, int x) {
    for (int i = 0; i < n; i++) {
        if (i == l) {
            continue;
        }
        if (mat[i][x]) {
            xor_line(i, l);
        }
    }
}

uint64_t solve() {
    int cnt = 0;
    for (int i = 0; i < SIZE; i++) {
        for (int j = cnt; j < n; j++) {
            if (mat[j][i]) {
                swap_line(cnt, j);
                do_gauss(cnt, i);
                cnt++;
                break;
            }
        }
    }
    uint64_t ans = 0;
    for (int i = 0; i < SIZE; i++) {
        uint64_t u = 0;
        for (int j = 0; j < SIZE; j++) {
            u = (u << 1) | mat[i][j];
        }
        ans ^= u;
    }
    return ans;
}

int main() {
    while (input(n)) {
        memset(mat, 0, sizeof(mat));
        for (int i = 0; i < n; i++) {
            uint64_t u = 0;
            input(u);
            for (int j = 0; j < SIZE; j++) {
                if (u & (1ULL << j)) {
                    mat[i][SIZE - j - 1] = 1;
                }
            }
        }
        print(solve());
    }
    return 0;
}
```
