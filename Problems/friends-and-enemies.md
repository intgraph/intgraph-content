[Metadata]
title: Friends and Enemies
date: 2015-10-05 20:51

[Tags]
difficulty: 4
categories: disjoint-set, heuristic algorithm
source: 51nod

[Description]

There is a country with N people. At the beginning, all the people here are independent individuals, that is, there are no friends or no enemies in the country.

And we, play as god, are able to change the relationship of two people at a single time. There are two operations we can do in this problem: (1) set friends (2) set enemies.

If two people is vaild to set as friends or enemies, print "YES" as an acknowlegement. Otherwise, print "NO" to show the operation is illegal.

There are at most 10^5 operations, and the identity numbers of people can up to 10^8.

Here is an example:

```
>> INPUT
3
1 2 1
1 3 1
2 3 0

<< OUTPUT
YES
YES
NO
```

The "3" in the first line shows that there are 3 operations in total.

And the first operation is to set people "1" and "2" as friends. Because the people are all independent at the very beginning. It's OK for that operation. So we print "YES".

The second operation is similar to the first one which is legal. We simply print "YES".

However, the intent of the last operation is to set "2" and "3" as enemies. Here we know that "1", "2" and "3" are friends. So the operation is illegal. We print "NO" as a result.

> Source: http://www.51nod.com/onlineJudge/questionCode.html#!problemId=1515

[Solution]

Let's simplify the problem. It's easy to find out we can use disjoint-set to detect if two people is in a same group or not. If people "a" and people "b" are in the same set, they are definetely friend. Otherwise, they may be strangers or enemies.

So the next thing we should do is try to find a way to detect the relationship of enemies.

It's also not hard to point out that a hash table is a good data strucature to store & search the hostile relationship.

But here comes the problem, as we have to merge the disjoint-set of friend relationship. The hostile relationship must be updated everytime the merging happens. By a simple calculation, we might find out that the time complexity is up to `O(n * n)`, which is not acceptable in this problem.

For example, "1" & "2" are friends, and "1" & "10", "2" & "11" are enemies. If we set "3" & ("1", "2") as friends, we have to set "3" & "10", "3" & "11" as enemies. If the relaionship is complex, it will take a long time to maintain the relationship.

The solution for the difficult situation is called "heuristic merging". It's an optimization of the disjoint-set. The principle of this algorithm is to merge the smaller set into the larger set. It will remarkably reduce the time compleixty for the worst case of the merging of disjoint-set.

In our problem, we intend to merge the group with less enemies into the group with more enemies. And the time complexity will be `O(n)` sum up all the merging operations.

The code is something like this:

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <map>
#include <set>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

struct Command {
    int x, y, p;
};

const int SIZE = 123456;

int n;
int father[SIZE];
vector<Command> cmds;
vector<set<int> > not_equal;

void init() {
    for (int i = 0; i < SIZE; i++) {
        father[i] = i;
    }
    not_equal.resize(SIZE);
}

int find_father(int x) {
    if (x == father[x]) {
        return x;
    }
    return father[x] = find_father(father[x]);
}

// crucial point: **heuristic algorithm**
void do_union(int a, int b) {
    if (not_equal[a].size() > not_equal[b].size()) {
        swap(a, b);
    }
    father[a] = b;
    for (auto v: not_equal[a]) {
        not_equal[b].insert(v);
        not_equal[v].erase(a);
        not_equal[v].insert(b);
    }
    not_equal[a].clear();
}

void solve() {
    init();
    for (auto cmd: cmds) {
        int x = find_father(cmd.x);
        int y = find_father(cmd.y);
        if (x > y) {
            swap(x, y);
        }
        int p = cmd.p;

        if (p == 1) {
            if (x == y) {
                puts("YES");
            } else if (not_equal[x].find(y) == not_equal[x].end()) {
                do_union(x, y);
                puts("YES");
            } else {
                puts("NO");
            }
        } else {
            if (x == y) {
                puts("NO");
            } else if (not_equal[x].find(y) == not_equal[x].end()) {
                not_equal[x].insert(y);
                not_equal[y].insert(x);
                puts("YES");
            } else {
                puts("YES");
            }
        }
    }
}

int main() {
    int a, b, c;
    input(n);
    map<int, int> mp;
    for (int i = 0; i < n; i++) {
        scanf("%d%d%d", &a, &b, &c);
        cmds.push_back({a, b, c});
        mp[a] = -1;
        mp[b] = -1;
    }

    int idx = 1;
    for (auto& p: mp) {
        p.second = idx++;
    }

    for (auto& cmd: cmds) {
        cmd.x = mp[cmd.x];
        cmd.y = mp[cmd.y];
    }

    solve();

    return 0;
}
```
