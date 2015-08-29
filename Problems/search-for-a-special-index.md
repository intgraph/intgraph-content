[Metadata]
title: Search for a Special Index
date: 2015-08-29 11:54:39

[Tags]
difficulty: 3
categories: binary search
source: Algorithms: A Creative Approach

[Description]

Given a sorted array of distinct number A[n]. Please determine whether there exists an index that `A[i] == i`.

[Solution]

Let us make some example first (assuming the first index is 1)

```
>> 1 2
<< YES

>> 2 3 4
<< NO

>> 0 3 7
<< NO

>> 0 2 10
<< YES
```

From these examples, we can get some hints:

* if a[i] > i, there is no such "special index" in sequence a[i...n]
* if a[i] < i, there is no such "special index" in sequence a[0...n]

That is, if `a[i] <= i && a[j] >= j`, there is possibility that there exists a "special index" in array `a[i...j]`.

Additionally, assuming we have two "special array" `a[i...j]` and `a[j...k]`. Accroding to the discussion above, there must be:
```
a[i] <= i && a[j] >= j && a[j] >= j && a[k] <= k
```

As a result, if we can concat these two "special arrays", the index `j` must be a `special index`. In another word, that is, if an array is `maybe specical array`, then either part of the biparting subarray is another `maybe special subarray`.

```python
from random import randint

def gen():
    n = randint(1, 1000)
    ns = [randint(-1000, 2000) for i in xrange(n)]
    ns = sorted(list(set(ns)))
    return ns

def solve(ns):
    # Contain only distinct number
    assert len(set(ns)) == len(ns)

    # The array is sorted
    assert sorted(ns) == ns

    l, r = 0, len(ns) - 1
    while l <= r:
        if ns[l] > l or ns[r] < r:
            return -1
        m = (l + r) >> 1
        if ns[m] == m:
            return m
        elif ns[m] < m:
            l = m + 1
        else:
            r = m - 1
    else:
        return -1

if __name__ == '__main__':
    for i in xrange(1000):
        ns = gen()
        idx = solve(ns)
        if idx == -1:
            for i, num in enumerate(ns):
                assert num != i
        else:
            assert ns[idx] == idx

```
