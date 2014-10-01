[Metadata]
title: maximum sum - 2
date: 2014-09-10 23:30:17 

[Tags]
categories: dp
difficulty: 3.5
source: POJ, baidu

[Description]
Give you N integers a1, a2 ... aN (|ai| <=1000, 1 <= i <= N). 

![formula](http://wizmann-pic.qiniudn.com/7c628ebf75525b633fc66e563d8d1b4c)

You should output S. 

### Input

> The input will consist of several test cases. For each test case, one integer N (2 <= N <= 100000) is given in the first line. Second line contains N integers. The input is terminated by a single line with N = 0.

```
5
-5 9 -5 11 20
0
```

### Output

> For each test of the input, print a line containing S.

```
40
```

link: http://poj.org/problem?id=2593

[Solution]
This problem request for the maximum sum of two intervals. This is quite similar to find ONE maximum interval.

```
S = max_interval(0...j) + max_interval(j + 1 ... n - 1)
```

And this is our final solution.

```cpp
// Result: wizmann	2593	Accepted	8456K	266MS	C++	1070B	2014-09-10 23:28:46
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

const int INF = 0x3f3f3f3f;

const int SIZE = 1000100;

int ltor[SIZE], rtol[SIZE];
int n, array[SIZE];

void calc(int* dp, int from, int to, int delta)
{
    int sum = -INF, t = 0;
    for (int i = from; i != to; i += delta) {
        t += array[i];
        sum = max(sum, t);
        dp[i] = sum;
        t = max(t, 0);
    }
}

int main()
{
    while (input(n) && n) {
        int ans = -INF;
        for (int i = 0; i < n; i++) {
            scanf("%d", array + i);
        }
        memset(ltor, 0, sizeof(ltor));
        memset(rtol, 0, sizeof(rtol));

        calc(ltor, 0, n, 1);
        calc(rtol, n - 1, -1,  -1);

        for (int i = 0; i + 1 < n; i++) {
            int l = ltor[i];
            int r = rtol[i + 1];
            ans = max(ans, l + r);
        }
        print(ans);
    }
    return 0;
}
```
