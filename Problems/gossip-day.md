[Metadata]
title: Gossip Day
date: 2014-11-06 00:42:52 

[Tags]
difficulty: 4.5
categories: dp, graph
source: unknown

[Description]

There are N students in a classroom, which have the number from 1 to N. You will be given  N of relationships with your friends. ($ 1 \le N \le 14$)

In this classroom, they have a routine they use to pass on information utilizing their contact network. 

* First, Student 1 will receive the information. At this stage, no one except for Student 1 knows about this information.
* The person who received the information will choose **one of his friends** who has not received it yet with equal probability then deliver the info to them.
* This process ends when all of your friends learned about the info.

Please output the **average** and the **expectation** of the number of people to deliver the information.

Errors are acceptable when the value of absolute errors or relative errors are no more than $10^{−6}$.

[Solution]

As the number of students is quite small, the intuitive thought of the problem is using the search algorithms to get the final answer. However, it will take a long time if the students are all friends, and the time complexity is `O(N!)` which is not acceptable if N is greater than 10.

Consider this situation, the gossip has known by a set of students S, and already passed to student P (P is in the set S). We mark this situation as dp[P][S], and it's easy to find out that if we move from this situation to another ones. And we deal with these situations that which can't make a transition, the answer is right here.

It's a typical **dynamic programming** problem, and quite similar as the one to find the **Hamilton path**. The code is right here.

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

typedef long long llint;

const int SIZE = 15;

struct node {
    llint cnt;
    double p;
    bool eop; // end of path
};

int n;
node dp[SIZE][1 << SIZE];
char visit[SIZE][1 << SIZE];
vector<int> g[SIZE];

void init() {
    memset(dp, 0, sizeof(dp));
    memset(visit, 0, sizeof(visit));
    for (int i = 0; i < SIZE; i++) {
        g[i].clear();
    }
}

void solve() {
    // pair<pos, status>
    queue<pair<int, int> > q;
    q.push({0, 1});
    dp[0][1] = {1, 1.0, false};
    while (!q.empty()) {
        auto p = q.front();
        q.pop();

        int pos = p.first, status = p.second;

        int num = 0;
        for (auto iter = g[pos].begin(); iter != g[pos].end(); ++iter) {
            int r = *iter;
            if (!(status & (1 << r))) {
                num++;
            }
        }
                
        bool flag = true;
        for (auto iter = g[pos].begin(); iter != g[pos].end(); ++iter) {
            int r = *iter;
            if (status & (1 << r)) {
                continue;
            }
            int next = status | (1 << r);
            dp[r][next].cnt += dp[pos][status].cnt;
            dp[r][next].p   += dp[pos][status].p / num;
            flag = false;
            if (!visit[r][next]) {
                q.push({r, next});
                visit[r][next] = 1;
            }
        }
        dp[pos][status].eop = flag;
    }
}

void get_answer() {
    double ave = 0, exp = 0;
    llint sum = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < (1 << n); j++) {
            if (!dp[i][j].eop) { continue; }
            int u = 0;
            for (int k = 0; k < n; k++) {
                if (j & (1 << k)) {
                    u++;
                }
            }
            sum += dp[i][j].cnt;
            exp += dp[i][j].p * u;
            ave += dp[i][j].cnt * u;
        }
    }
    ave /= sum;
    printf("AVE: %.10lf\n", ave);
    printf("EXP: %.10lf\n", exp);
}

int main()
{
    freopen("input.txt", "r", stdin);
    string instr;
    while (input(n)) {
        init();
        for (int i = 0; i < n; i++) {
            input(instr);
            for (int j = 0; j < n; j++) {
                if (instr[j] == 'Y') {
                    g[i].push_back(j);
                }
            }
        }
        solve();
        get_answer();
    }
    return 0;
}
```
