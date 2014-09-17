[Metadata]
title: partition the string into palindrome
date: 2014-09-16 14:18:00

[Tags]
difficulty: 3.5
categories: palindrome, dp
source: 待字闺中

[Description]
Given a string, you have to partition it into palindromes. 

For example, the string "ababbbabbababa" can be partitioned into  "aba|b|bbabb|a|b|aba" which contains six palindromes. (p.s. a only single characters is a palindrome)

You task is to find the minimal time to partition the string into palindromes.

Here are some examples:

```
Input  > ababbbabbababa
Output > 4
// a|babbbab|b|ababa, partition 3 times

Input  > x
Output > 0
// x, no need to partition

[Solution]
Assuming we have a string S, and ``S[0...i]`` can be cut into k palindromes.

And here, if S[i + 1...j] is a palindrome, ``S[0...j]`` can be cut into k + 1 palindromes, and it's easy to see.

The formula can be represent in this way.

```
p_time[0...j] = min(p_time[0...i] + 1 | if S[i + 1...j] is palindrome)
```

Our next problem is, how to tell if a string is palindrome.

The easiest and brute force way is to check every sub-string by a scan algorithm. And the time complexity is O(n^3) which will take a long time for a long string.

And the manacher algorithm, is not suitable for this problem because it only can find the longest palindrome in the string.

Here we take another dp algorithm, to find all the palindromes with O(n^2) time complexity.

```
isPalindrome[i][j] = i == j || (S[i] == S[j] && isPalindrome[i+1][j-1]);
```

And here is my code.

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <algorithm>
#include <iostream>
#include <vector>
#include <cmath>
#include <set>
#include <queue>
#include <cassert>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

// not tested
const int INF = 0x3f3f3f3f;
const int SIZE = 128;
int isPalin[SIZE][SIZE];
int p_time[SIZE];

int solve(const char* S, int n)
{
    memset(isPalin, 0, sizeof(isPalin));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j + i < n; j++) {
            int a = j, b = j + i;
            if (S[a] != S[b]) {
                continue;
            }
            if (a + 1 >= b - 1 || isPalin[a + 1][b - 1]) {
                isPalin[a][b] = 1;
            }
        }
    }
    memset(p_time, INF, sizeof(p_time));
    for (int i = 0; i < n; i++) {
        for (int j = i; j >= 0; j--) {
            if (isPalin[j][i]) {
                int u = j - 1 < 0? 0: p_time[j - 1];
                p_time[i] = min(p_time[i], u + 1);
            }
        }
    }
    return p_time[n - 1];
}

int main()
{
    string s;

    s = "ababbbab";
    print(solve(s.c_str(), s.length()));

    s = "abababa";
    print(solve(s.c_str(), s.length()));

    s = "a";
    print(solve(s.c_str(), s.length()));

    s = "ababbbabbababa";
    print(solve(s.c_str(), s.length()));
    return 0;
}
```
