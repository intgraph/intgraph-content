[Metadata]
title: Interleaving String - 2
date: 2014-12-07 21:57:14 

[Tags]
difficulty: 4
categories: recursion
source: mitbbs, google

[Description]

Given a string which length is N (N % 2 == 0). Your mission is to merge the first half and the second half of the string into a new interleaving string.

For example, the result of merging ``abc123`` is ``a1b2c3``.

Additionally, no extra memory space is allowed.

[Solution]

If we can use an extra memory space, we can easily solve the problem.

```python
s = "abc123"
print ''.join(reduce(lambda x, y: x + y, zip(s[:len(s)/2], s[len(s)/2:])))
```

However, we need something new to show off talent.

Consider this, if we have already got the idea of interleaving string A1 and B1. And then, we expand the string A1 and B1. That is, ``A = A1 + A2``, ``B = B1 + B2``. It's easy to find out that ``Interleaving(A, B) = Interleaving(A1, B1) + Interleaving(A2, B2)``.

As a result, by every step, we can make the problem ``Interleaving(A, B)`` into a same sub-problem. And if we keep doing the partition and swap stuff, it's easy to get the final answer.

The time complexity of the algorithm is O(n * logn), as the string could be partitioned for `O(logn)` times, and an `O(n)` time is needed to deal with all the sub-problem.

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <string>
#include <cassert>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

const int SIZE = 1024;
char str[SIZE];

void do_interleave(char* istr, int len) {
    if (len <= 2) {
        return;
    }

    int half = len / 2;
    if (half % 2 != 0) {
        for (int i = half; i < len - 1; i++) {
            swap(istr[i - 1], istr[i]);
        }
        do_interleave(istr, len - 2);
        return;
    }

    int p = half / 2, q = half;
    while (p < half) {
        swap(istr[p], istr[q]);
        p++; q++;
    }
    do_interleave(istr, len / 2);
    do_interleave(istr + len / 2, len / 2);
}

char* interleave(char* istr, int len) {
    do_interleave(istr, len);
    return istr;
}

int main() {
    while (input(str)) {
        int len = strlen(str);
        assert(len % 2 == 0);
        print(interleave(str, len));
    }
    return 0;
}
```
