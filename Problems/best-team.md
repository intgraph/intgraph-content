[Metadata]
title: Best Team
date: 2015-10-28 01:47:30

[Tags]
difficulty: 4
categories: monotone
source: codeforces, 51nod

[Description]

There are N bears standing in a line. Your mission is to pick a group of bears to make a team. A group of bears is a non-empty contiguous segment of the line.  The size of a group is the number of bears in that group. The strength of a group is the minimum height of the bear in that group.

The input will be the strengths of each bear. And the output should be the largest strengths for the teams of size `[1...n]`.

```
>> Sample Input
10
1 2 3 4 5 4 3 2 1 6
<< Sample Output
6 4 4 3 3 2 2 1 1 1
```

Link: [Codeforces 547B](http://codeforces.com/problemset/problem/547/B)

[Solution]

For a team with size K, we can use a [monotone priority queue](https://en.wikipedia.org/wiki/Monotone_priority_queue) to maintain the minimal strength of a team, and finally get the answer.

However, this problem is much harder. Because we have to find out all the largest strength for all the teams with different size.

The solution of this problem is still the monotone queue. The element we keep in the queue is the strength value of a bear as well as the range that this bear is of the minimal strength. And when the element is popped out from the queue, we maintain a result array which the `res[i]` stands for the maximum of the strength of a group with size smaller than `i`.


```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <stack>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

struct Node {
    int l;
    int r;
    int value;
};

int n;
int nums[SIZE];
int res[SIZE];
vector<Node> st;

int main() {
    input(n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &nums[i]);
    }
    nums[n] = -1;

    for (int i = 0; i <= n; i++) {
        int pre = i;
        while (!st.empty() && nums[i] < st.rbegin()->value) {
            // oh, it's my fault?
            int d = i - 1 - st.rbegin()->l;
            res[d] = max(res[d], st.rbegin()->value);
            pre = st.rbegin()->l;
            st.pop_back();
        }

        st.push_back({pre, i, nums[i]});
    }

    for (int i = n - 2; i >= 0; i--) {
        res[i] = max(res[i], res[i + 1]);
    }

    for (int i = 0; i < n; i++) {
        printf("%d ", res[i]);
    }
    puts("");

    return 0;
}

```
