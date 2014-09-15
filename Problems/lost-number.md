[Metadata]
title: lost number
date: 2014-09-15 23:23:47 

[Tags]
difficulty: 3.5
categories: radix-sort, in-place algorithm
source: 待字闺中

[Description]
Given an arbitaray array A. Please find the minimal number in the range ``[0 ... n-1]`` which doesn't appear in the array A.

The algorithm with O(n) time and O(1) extra memory space is excellent.

[Solution]
The numbers which greater than ``n`` or less than ``0`` is not the answer apparently.

If there are ``0`` to ``n - 1`` in the array, so the answer there is ``n``.

And if there's a "hole" in the range ``[0 ... n-1]``. The first "hole" in the array is the final answer.

The solution there is do the in-place radix sort first, which takes O(n) time. And then, we scan the whole array to find the "hole". At last, the first "hole" or the number ``n`` is the final answer.

```cpp
// random data tested
int lost_number(int n, vector<int>& vec)
{
    // in-place radix sort
    for (int i = 0; i < n; i++) {
        int p = i;
        while (vec[p] != p && vec[p] >= 0 && vec[p] < n) {
            int u = vec[p];
            if (vec[u] == vec[p]) {
                break;
            }
            swap(vec[u], vec[p]);
        }
    }
    int lost = n;
    for (int i = 0; i < n; i++) {
        if (vec[i] != i) {
            lost = i;
            break;
        }
    }
    return lost;
}
```

The tester.

```python
from random import randint, shuffle

for i in xrange(1, 100):
    n = i
    ns = range(i)
    shuffle(ns)
    print n
    print ' '.join(map(str, ns))
    print i # <= answer

for i in xrange(100):
    n = randint(1, 1000)
    ns = [randint(-100, 100) for i in xrange(n)]
    v = min(filter(lambda x: x >= 0, list(set(ns) ^ set(range(n)))))
    print n
    print ' '.join(map(str, ns))
    print v # <= answer
```

