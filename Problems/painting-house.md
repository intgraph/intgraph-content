[Metadata]
title: Painting House
date: 2015-08-15 13:33:08

[Tags]
difficulty: 3
categories: dp
source: Leetcode, Facebook, linkedin

[Description]

There are a row of houses, each house can be painted with three colors red, blue and green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.
You have to paint the houses with minimum cost. How would you do it?

Note: Painting house-1 with red costs different from painting house-2 with red.  The costs are different for each house and each color.

[Solution]

Simple Problem with DP. Be ware of all the details with potential mistakes.

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <climits>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

enum {
    RED = 0,
    BLUE = 1,
    GREEN = 2,
    COLOR_NUM,
};

class Solution {
public:
    int minCost(int n, const vector<vector<int> >& cost) {
        vector<int> dp[2];
        dp[0] = vector<int>(COLOR_NUM, 0);
        dp[1] = vector<int>(COLOR_NUM, 0);
        int ptr = 0;

        for (int i = 0; i < n; i++) {
            int next = ptr ^ 1;
            for (int j = 0; j < COLOR_NUM; j++) {
                dp[next][j] = numeric_limits<int>::max();
                for (int k = 0; k < COLOR_NUM; k++) {
                    if (j == k) {
                        continue;
                    }
                    dp[next][j] = min(dp[next][j], dp[ptr][k] + cost[k][i]);
                }
            }
            ptr = next;
        }
        int ans = numeric_limits<int>::max();
        for (int i = 0; i < COLOR_NUM; i++) {
            ans = min(ans, dp[ptr][i]);
        }
        return ans;
    }
};

int main() {
    vector<vector<int> > cost = {
        {7, 3, 8, 6, 1, 2},
        {5, 6, 7, 2, 4, 3},
        {10, 1, 4, 9, 7, 6}
    };
    Solution S;
    int ans = S.minCost(6, cost);
    print(ans);
    return 0;
}
```
