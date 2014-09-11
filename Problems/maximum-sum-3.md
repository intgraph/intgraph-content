[Metadata]
title: maximum sum - 3
date: 2014-09-11 00:20:32 

[Tags]
categories: dp
difficulty: 3.5
source: hduoj, baidu

[Description]
Given a two-dimensional array of positive and negative integers, a sub-rectangle is any contiguous sub-array of size 1 x 1 or greater located within the whole array. The sum of a rectangle is the sum of all the elements in that rectangle. In this problem the sub-rectangle with the largest sum is referred to as the maximal sub-rectangle.

As an example, the maximal sub-rectangle of the array:

```
0 -2 -7 0
9 2 -6 2
-4 1 -4 1
-1 8 0 -2
```

is in the lower left corner:

```
9 2
-4 1
-1 8
```

and has a sum of 15.

link: http://acm.hdu.edu.cn/showproblem.php?pid=1081

[Solution]
A vintage problem, too. We sum up to column i to column, and then to find the max interval, so that we can find out the max sub-matrix.

```cpp
// Result: 2014-09-11 00:35:46	Accepted	1081	0MS	412K	1101 B	G++	Wizmann
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
const int SIZE = 128 + 5;

int g[SIZE][SIZE];
int array[SIZE];
int n;

int max_interval(int* dp)
{
    int ans = -INF, sum = 0;
    for (int i = 0; i < n; i++) {
        sum += dp[i];
        ans = max(ans, sum);
        sum = max(sum, 0);
    }
    return ans;
}

int main()
{
    while (input(n)) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                scanf("%d", &g[i][j]);
            }
        }

        int ans = -INF;
        for (int i = 0; i < n; i++) {
            memset(array, 0, sizeof(array));
            for (int j = i; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    array[k] += g[k][j];
                }
                ans = max(ans, max_interval(array));
            }
        }
        print(ans);
    }
    return 0;
}
```
