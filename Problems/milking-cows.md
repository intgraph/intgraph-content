[Metadata]
title: Milking Cows
date: 2015-04-08 00:16:04 

[Tags]
difficulty: 2.5
categories: greedy, interval, sorting
source: The Algorithm Design Manual

[Description]

There are serval cows and several workers in a farm. One worker begins to milk a cow at time `a` and ends at time `b`.

Now your are given all pairs of `a` and `b` which indicate the work time of all the workers. Please find out:

* The longest time interval at least one cow was milked.
* The longest time interval (after milking starts) during which no cows were being milked.

[Solution]

This problem is a variety of "Longest non-intersect intervals". Greedy is the way to solve it.

Firstly, we sort the pairs with key `a`. And then, we will use the induction way to prove the correctness.

Assuming we've already got a working period `<ai, bi>`, which `ai` is the earliest one among the rest periods. We call it "earliest period".

If this period doesn't not intersect with any other period which is available, we can confirm that period is a **complete working period**. 

And then, we keep adding new periods to "earliest period" if we get intersection or clear the "earliest period" if the periods is not overlap. Eventually, it's easy to find out the longest working period. And the idle period is quite the same.

The implementation is right here:

```cpp
// Result: Wizmann	181	Accepted	1080 KB	7 MS	C++	1015 B	
2015-04-07 23:27:49
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

vector<pair<int, int> > vec;

int main() {
    int n;
    int a, b;
    
    while (input(n)) {
        vec.clear();
        for (int i = 0; i < n; i++) {
            input(a >> b);
            vec.push_back({a, b});
        }
        sort(vec.begin(), vec.end());

        int busy = 0;
        int idle = 0;
        int start = vec.begin()->first, end = vec.begin()->second;
        for (auto& p: vec) {
            a = p.first;
            b = p.second;

            if (a > end) {
                idle = max(idle, a - end);
                start = a;
                end = b;
                busy = max(busy, end - start);
            } else {
                end = max(end, b);
                busy = max(busy, end - start);
            }
        }

        printf("%d %d\n", busy, idle);
    }
    return 0;
}
```

You can test your code [here][1].

[1]: http://acm.uestc.edu.cn/#/problem/show/181
