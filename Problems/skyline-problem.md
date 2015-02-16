[Metadata]
title: Skyline Problem
date: 2014-10-01 23:07:29 

[Tags]
difficulty: 4
categories: segment-tree, divide-and-conquer, data structures
source: Algorithms: A Creative Approach

[Description]

The skyline of the building is beautiful in the dusk.

![skyline](http://wizmann-pic.qiniudn.com/c0732c64bb2be7c5c90bee4781da3f38)

Our problem is to find the area of the skyline of the building. Assuming the buildings are all rectangular.

![problem](http://wizmann-pic.qiniudn.com/e90d939871bf1c60ef0128ba380fdf60)

There are n buildings in the city. And the 

The input will give the tuple ``(l, r, h)`` which indicates the left and right coordinates and the height of the building. (``1 <= l <= r <= n``)

You should output the area of these skylines.

[Solution]

The brute force is intuitive, which time complexity is O(n^2).

```python
# not tested
n = int(raw_input())
ll = [0 for i in xrange(n + 1)]
for i in xrange(n):
    (l, r, h) = map(int, raw_input().split())
    for j in xrange(l, r + 1):
        ll[j] = max(ll[j], h)
print sum(ll)
```

It's easy for us to know that to merge two skyline into one will take O(n) time. And do it in divide-and-conquer way, divide every problem of n into two sub-problem of n/2 will solve the problem in O(n * logn) time.

```cpp
void divide_and_conquer(int l, int r, vector<int>& vec) {
    if (l == r) {
        for (int i = buildings[l].l; i <= buildings[l].r; i++) {
            vec[i] = buildings[l].h;
        }
        return;
    }
    int m = (l + r) >> 1;
    vector<int> vec_a, vec_b;
    vec_a.resize(n + 1);
    vec_b.resize(n + 1);
    divide_and_conquer(l, m, vec_a);
    divide_and_conquer(m + 1, r, vec_b);
    for (int i = 0; i <= n; i++) {
        vec[i] = max(vec_a[i], vec_b[i]);
    }
}
```

This algorithm is faster, but not fast enough, because it will take O(n * logn) memory and in this code, the meomory are allocated dynamicly, which is very slow.

The [segment tree](http://en.wikipedia.org/wiki/Segment_tree) is a faster algorithm. The space complexity is O(n) and can be allocate in a static way, and the time complexity is O(n*logn) in average.

The segment tree algorithm runs five times faster (or more) than the divide-and-conquer one on my i5 computer. But the code here is complex and error-prone. But if you can make it fast and bug-free, it will get you a bouns point in the interview.

```cpp
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <iostream>
#include <algorithm>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

const int SIZE = 1024;

inline int LEFT(int x)  { return 2 * x + 1; }
inline int RIGHT(int x) { return 2 * x + 2; }

struct SegTree {
    struct node { int l, r, v; };
    node stree[SIZE * 4];
    int idx;
    
    void init(int l, int r) {
        idx = 0;
        do_init(l, r, 0);
    }

    void do_init(int l, int r, int pos) {
        stree[pos] = {l, r, 0};
        if (l == r) {
            return;
        }
        int m = (l + r) >> 1;
        do_init(l, m, LEFT(pos));
        do_init(m + 1, r, RIGHT(pos));
    }

    void update(int l, int r, int v) {
        do_update(l, r, v, 0);
    }

    void do_update(int l, int r, int v, int pos) {
        if (l == stree[pos].l && r == stree[pos].r && stree[pos].v != -1) {
            stree[pos].v = max(v, stree[pos].v);
            return;
        }
        int m = (stree[pos].l + stree[pos].r) >> 1;
        if (stree[pos].v != -1) {
            do_update(stree[pos].l, m, stree[pos].v, LEFT(pos));
            do_update(m + 1, stree[pos].r, stree[pos].v, RIGHT(pos));
            stree[pos].v = -1;
        }
        if (r <= m) {
            do_update(l, r, v, LEFT(pos));
        } else if (l > m) {
            do_update(l, r, v, RIGHT(pos));
        } else {
            do_update(l, m, v, LEFT(pos));
            do_update(m + 1, r, v, RIGHT(pos));
        }
        if (stree[LEFT(pos)].v == stree[RIGHT(pos)].v) {
            stree[pos].v = stree[LEFT(pos)].v;
        }
    }

    int sumup() {
        return do_sumup(0);
    }

    int do_sumup(int pos) {
        if (stree[pos].v != -1) {
            return (stree[pos].r - stree[pos].l + 1) * stree[pos].v;
        } else {
            return do_sumup(LEFT(pos)) + do_sumup(RIGHT(pos));
        }
    }
};

SegTree st;

int main()
{
    int n;
    int a, b, c;
    while (input(n)) {
        st.init(1, n);
        for (int i = 0; i < n; i++) {
            input(a >> b >> c);
            st.update(a, b, c);
        }
        print (st.sumup());
    }
    return 0;
}
```

