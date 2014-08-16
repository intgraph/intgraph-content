[Metadata]
title: Interleaving String
date: 2014-08-16 17:14:25 

[Tags]
difficulty: 3
categories: dp
source: Leetcode, itint5, google

[Description]
Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

For example,
Given:
s1 = "aabcc",
s2 = "dbbca",

When s3 = "aadbbcbcac", return true.
When s3 = "aadbbbaccc", return false.

[Solution]
It's a basic dynamic programming problem.

We can get a formula like that.

```
dp[i][j] |= str1[i] == str3[i + j]? dp[i - 1][j];
dp[i][j] |= str2[i] == str3[i + j]? dp[i][j - 1];
```

And here it is.

```cpp
int dp[2048][2048]; // it's not right to guess the scale of the input, but who cares :)

bool isInterleaving(string str1, string str2, string str3) {
    if (str1.length() + str2.length() != str3.length()) {
        return false;
    }
    if (str1.empty() || str2.empty()) {
        return str3 == str1 + str2;
    }
    memset(dp, 0, sizeof(dp));
    dp[0][0] = 1;
    for (int i = 0; i <= str1.length(); i++) {
        for (int j = 0; j <= str2.length(); j++) {
            if (i - 1 >= 0 && str1[i - 1] == str3[i + j  - 1]) {
                dp[i][j] |= dp[i - 1][j];
            }
            if (j - 1 >= 0 && str2[j - 1] == str3[i + j - 1]) {
                dp[i][j] |= dp[i][j - 1];
            }
        }
    }
    return dp[str1.length()][str2.length()];
}
```
