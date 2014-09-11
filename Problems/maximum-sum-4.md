[Metadata]
title: maximum sum - 4
date: 2014-9-11 13:43:00

[Tags]
categories: dp
difficulty: 4
source: hduoj

[Description]
Given a consecutive number sequence S1, S2, S3, S4 ... Sx, ... Sn (1 ≤ x ≤ n ≤ 1,000,000, -32768 ≤ Sx ≤ 32767). We define a function sum(i, j) = Si + ... + Sj (1 ≤ i ≤ j ≤ n).

Now given an integer m (m > 0), your task is to find m pairs of i and j which make sum(i1, j1) + sum(i2, j2) + sum(i3, j3) + ... + sum(im, jm) maximal (ix ≤ iy ≤ jx or ix ≤ jy ≤ jx is not allowed).

### Input
Each test case will begin with two integers m and n, followed by n integers S1, S2, S3 ... Sn.
Process to the end of file.

```
1 3 1 2 3
2 6 -1 4 -2 3 -2 3
```

### Output
Output the maximal summation described above in one line.

```
6
8
```

link: http://acm.hdu.edu.cn/showproblem.php?pid=1024

[Solution]
It's a much harder problem comparing to the previous problems, because we have to find multiple maximum intervals rather than the ONLY one (or two).

Assuming we have already get the maximum sum of j intervals which includes the number i, it number is represent by dp[j][i].

As a result, ``dp[j][i + 1] = dp[j][i] + array[i + 1]`` and ``dp[j + 1][i] = max(dp[j][0 ... i - 1]) + array[i]``.

This is the main formula of this problem. But some detail and pitfalls must be pay some attention to.

For example, the initial value of the answer can't be 0, because the summary of some interval may be negative number. And the numbers ``dp[j][0...j - 1]`` are not exist because you can separate a sequence of less than j numbers into j parts.

Blahblahblah... Be aware of your codes, and you can get an "Accepted" eventually. 

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

typedef long long llint;

const int INF = 0x3f3f3f3f;
const int SIZE = 1000001;
int dp[2][SIZE];
int array[SIZE];
int n, m;

int solve()
{
    int ptr = 0;
    memset(dp, 0, sizeof(dp));
    for (int i = 0; i < m; i++) {
        int maxi = dp[ptr ^ 1][i - 1];
        dp[ptr][i] = dp[ptr ^ 1][i - 1] + array[i];
        for (int j = i + 1; j <= n - m + i; j++) {
            maxi = max(maxi, dp[ptr ^ 1][j - 1]);
            dp[ptr][j] = max(
                    dp[ptr][j - 1] + array[j],
                    maxi + array[j]);
        }
        ptr ^= 1;
    }
    return ptr ^ 1;
}

int main()
{
    while (scanf("%d%d", &m, &n) != EOF) {
        for (int i = 0; i < n; i++) {
            scanf("%d", array + i);
        }
        int ptr = solve();
        int ans = -INF;
        for (int i = m - 1; i < n; i++) {
            ans = max(ans, dp[ptr][i]);
        }
        print(ans);
    }
    return 0;
}
```
