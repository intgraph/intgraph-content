[Metadata]
title: String to Palindrome
date: 2014-09-01 16:05:00

[Tags]
difficulty: 3.5
categories: string, dp, dfs, loop invariant
source: 待字闺中

[Description]
There is an arbitrary string, your mission is to make the string into a palindrome by insert some characters to it.

Find out the minimum number of characters need to insert to make a palindrome.

Example:

```
>> ab
<< *b*ab

>> aa
<< aa

>> abc
<< abc*ba*
```

[Solution]
It's easy for us to know that, for a string which is palindrome, if we remove the first and the last characters, the rest of the string is a palindrome too.

In another word, if we have a palindrome, we can add two same characters to the beginning and the end of the string to make a new palindrome.

There are two ways to solve this problem which based on the two conclusion below.

The first method is to do the removal operation. If the first and the last characters are not the same, we have to insert one character at whether the beginning or end of the string, and do the removal job. The operation repeats until the string is empty, and then we count the number of characters that inserted.

The second method is treat the empty string as palindrome, and it's right of course. And we take steps to add two characters every time to extend the palindrome. If we can make these two characters the same, we have to insert an additional one.

```cpp
#include <cstdio>
#include <iostream>
#include <string>
#include <cstring>
#include <cassert>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

const int SIZE = 512;
const int INF = 0x3f3f3f3f;

int dp[SIZE][SIZE];

int sol_one(const char* str, size_t n)
{
    memset(dp, 0, sizeof(dp));
    for (int i = 1; i < n; i++) {
        for (int j = 0, k = j + i; j < n && k < n; j++, k++) {
            char ll = str[j], rr = str[k];
            dp[j][k] = INF;
            if (ll == rr) {
                dp[j][k] = min(dp[j][k], dp[j + 1][k - 1]);
            } else {
                dp[j][k] = min(dp[j][k], dp[j + 1][k] + 1);
                dp[j][k] = min(dp[j][k], dp[j][k - 1] + 1);
            }
        }
    }
    return dp[0][n - 1];
}

int do_sol_two(const char* str, int l, int r)
{
    if (l >= r) {
        return 0;
    }
    if (dp[l][r] != -1) {
        return dp[l][r];
    }
    if (str[l] == str[r]) {
        return dp[l][r] = do_sol_two(str, l + 1, r - 1);
    }
    return dp[l][r] = min(do_sol_two(str, l + 1, r), do_sol_two(str, l, r - 1)) + 1;
}

int sol_two(const char* str, size_t n)
{
    memset(dp, -1, sizeof(dp));
    return do_sol_two(str, 0, n - 1);
}

int main()
{
    string str;
    while (input(str)) {
        int a = sol_one(str.c_str(), str.length());
        int b = sol_two(str.c_str(), str.length());
        print(a << ' ' << b);
        assert(a == b);
    }
    return 0;
}

```

Personally, I prefer the second method because it's more clear and not that easy to make a misake. But in practise, the recursion is usually slower than the iteration. But who ever cares. :)

