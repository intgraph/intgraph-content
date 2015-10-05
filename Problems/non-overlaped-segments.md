[Metadata]
title: Non-overlaped segments
date: 2015-09-19 20:14:46

[Tags]
difficulty: 3
categories: sorting, stack
source: 51nod

[Description]

Given a batch of segments, please find the largest subset of the segments which are not overlaped.

The definition of overlap is for segment "[a, b]" and "[c, d]", there is always `(c - a) * (c - b) >= 0 && (d - a) * (d - b) >= 0`. That is, "[1, 2]" and "[2, 3]" is not overlaped, at time same time "[1, 3]" and "[2, 3]" is overlaped.

```
Sample Input
>> [1, 5], [2, 3], [3, 6]

Sample Output
<< 2
```

[Solution]

For the "segment problems", there are two things you need to do before you try to give the final solution.

First one is sorting. Because the segments are on the same axis, and the order is of crucial important for them.

Second one is find the right induction hypothesis. In this problem, the induction hypothesis is:

> We know how to solve the problem for the set of (sorted) segments [1...n]

The basic case is trivial, because it's easy to handle only one segment. Assuming we've got a **non-overlapped** segments set, add we are trying to add other one to it. There are two scenarios:

1. the new commer is non-overlapped with the last element of the set, then we just simply add it to the sets
2. the new commer is overlapped with the last one in the set.
    * because the segments is sorted, so the new comer will only overlapped with the last one.
    * then if the end point of the new comer is on the left side of the end point of the last element, we just replace it by the new one

And it's easy to prove the correctness of the solution. So I just skip it and post my code here.

```cpp
// http://www.51nod.com/onlineJudge/questionCode.html#!problemId=1133
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <set>
#include <limits>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

vector<pair<int, int> > lines;

int main() {
    int n;
    int a, b;
    input(n);

    for (int i = 0; i < n; i++) {
        input(a >> b);
        lines.push_back({a, b});
    }

    auto cmp = [](const pair<int, int>& pa, const pair<int, int>& pb) {
        return pa.first < pb.first;
    };

    sort(lines.begin(), lines.end(), cmp);
    int last = numeric_limits<int>::min();
    int cnt = 0;

    for (auto line: lines) {
        int sp = line.first;
        int ep = line.second;

        if (sp >= last) {
            cnt++;
            last = ep;
        } else {
            last = min(last ,ep);
        }
    }
    print(cnt);
    return 0;
}
```
