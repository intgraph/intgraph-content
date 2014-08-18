[Metadata]
title: No friends on the bus
date: 2014-08-18 23:02:26 

[Tags]
difficulty: 4
categories: constructive algorithms, combinatorics
source: codeforces

[Description]
There's a school which have k buses and n students. The school planned to take the students to d different places for d days.

Your mission is to arrange the students in the buses so that no two students become close friends. Two students will become close friends if and only if they are in the same buses for all d days.

Input
The first line of input contains three space-separated integers n, k, d (1 ≤ n, d ≤ 1000; 1 ≤ k ≤ 1e9).

Output
If there is no valid arrangement just print -1. Otherwise print d lines, in each of them print n integers. The j-th integer of the i-th line shows which bus the j-th student has to take on the i-th day. You can assume that the buses are numbered from 1 to k.

Sample Input and Output

```
>> input
3 2 2
<< output
1 1 2 
1 2 1 
```

```
>> input
3 2 1
<< output
-1
```

Link: http://codeforces.com/contest/459/problem/C

[Solution]
It's easy for us to know that there are ``k ^ d`` different sequences in all for the students as no two students can take the same bus for all d days.

And then, our problem is how to make that sequence. You may notice that the sequence is so much like "n d-digits numbers in k-based numbers" as no two students share the same sequence, and there are ``k ^ d`` different sequences in total.

It's easy to describe, but truly hard to figure out when you are at an interview or participating a contest.

The luckiest thing is the principle of this solution is simple and easy to remember. So keep it in your mind, and you may use it some day.

The solution with O(n * d) extra memory space is just like this. (A score 80 solution)

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

typedef long long llint;

const int SIZE = 1024 + 233; // <= Do you know why?

int nums[SIZE][SIZE];

llint n, k, d;

int main()
{
    input(n >> k >> d);
    memset(nums, 0, sizeof(nums));

    int overflow = 0;
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < d; j++) {
            nums[i][j] = nums[i - 1][j];
        }
        overflow = 1;
        for (int j = 0; j < d; j++) {
            nums[i][j] += overflow;
            overflow = nums[i][j] / k;
            nums[i][j] %= k;
        }
        if (overflow) {
            break;
        }
    }
    if (overflow) {
        print(-1);
    } else {
        for (int i = 0; i < d; i++) {
            for (int j = 0; j < n; j++) {
                printf("%d ", nums[j][d - i - 1] + 1);
            }
            puts("");
        }
    }
    return 0;
}
```

And a shorter and tricky solution with no extra memory space is just like below. (A score 100 solution)

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

typedef long long llint;

const int SIZE = 1024 + 233;

llint n, k, d;

int main() 
{
    input(n >> k >> d);
    if (pow(k, d) + 0.5 < n) {
        // <= if k ^ d is too large, the pow(k, d) will equal to inf
        // <= do you know why I plus 0.5 on the left side of the comparison?
        print(-1);
    } else {
        for (int i = 0; i < d; i++) {
            int v = 0, cnt = 0;
            int thre = (int)(pow(k, i) + 0.5);
            // <= the same pow and the same 0.5
            for (int j = 0; j < n; j++) {
                if (cnt == thre) {
                    v = (v + 1) % k;
                    cnt = 0;
                }
                printf("%d ", v + 1);
                cnt++;
            }
            puts("");
        }
    }
    return 0;
}
```
