[Metadata]
title: maximum sum - 1
date: 2014-09-10 23:11:37 

[Tags]
categories: dp
difficulty: 3
source: baidu, hduoj

[Description]
Given a sequence a[1],a[2],a[3]......a[n], your job is to calculate the max sum of a sub-sequence.

For example, given (6,-1,5,4,-7), the max sum in this sequence is 6 + (-1) + 5 + 4 = 14.

### Input:

> The first line of the input contains an integer T(1<=T<=20) which means the number of test cases. Then T lines follow, each line starts with a number N(1<=N<=100000), then N integers followed(all the integers are between -1000 and 1000).

```
2
5 6 -1 5 4 -7
7 0 6 -1 1 -6 7 -5
```

### Output:

> For each test case, you should output two lines. The first line is "Case #:", # means the number of the test case. The second line contains three integers, the Max Sum in the sequence, the start position of the sub-sequence, the end position of the sub-sequence. If there are more than one result, output the first one. Output a blank line between two cases.

```
Case 1:
14 1 4

Case 2:
7 1 6
```

link: http://acm.hdu.edu.cn/showproblem.php?pid=1003

[Solution]
This is a vintage problem. And it's easy to find out that the maximum sum for ``a[i] ... a[j]`` is equal to ``max(a[i] + ... a[j - 1]) + a[j]``. And this is how we solve this problem.

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

const int INF = 0x3f3f3f3f;

int n;
vector<int> vec;

void solve(int& sum, int& a, int& b)
{
    a = b = 0;
    sum = -INF;
    int t = 0, l = 0, r = 0;
    for (int i = 0; i < n; i++) {
        t += vec[i];
        if (t > sum) {
            r = i;
            a = l; b = r;
            sum = t;
        }
        if (t < 0) {
            t = 0;
            l = i + 1;
        }
    }
}

int main()
{
    int T, a, b, sum, cas = 1;
    input(T);
    while (T--) {
        input(n);
        vec.clear();
        for (int i = 0; i < n; i++) {
            scanf("%d", &a);
            vec.push_back(a);
        }
        solve(sum, a, b);
        printf("Case %d:\n", cas++);
        printf("%d %d %d\n", sum, a + 1, b + 1);
        if (T) {
            puts(""); // <= pitfall here
        }
    }
    return 0;
}
```
