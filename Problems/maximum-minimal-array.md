[Metadata]
title: maximum minimal array
date: 2014-09-14 11:06:05 

[Tags]
categories: sort, STL, thinking in reverse
difficulty: 3.5
source: unknown

[Description]
Given an array A, your task is to find a new array B that satisfies:

![formula-1](http://wizmann-pic.qiniudn.com/e757e3e4535ff0cfa2938f836c5e901d)

Further, you also have to find a new array C that satisifies:

![formula-2](http://wizmann-pic.qiniudn.com/eab6d02f98b92c3c4284b1338fe0dcca)

Solve the problem with a lower time/space complexity is prefered, and try to prove your algorithm is correct.

[Solution]
## Problem 1

The crucial task for this problem is to find a way to find the **maxmium minimal value**, and finding one maximum minimal value couldn't have a time complexity less than O(logn).

It's because if you ever find a faster algorithm, you can optimize the sort algorithm, and this will change the world. (Wow~)

The solution of the problem is like this. And we can do it in place if we like.

```cpp
// not tested
void solve(vector<int>& A) {
    set<int> st;
    size_t n = A.size();
    for (size_t i = 0; i < n; ++i) {
        auto iter = st.insert(A[i]).first;
        if (iter == st.begin()) {
            A[i] = -1;
        } else {
            A[i] = *(--iter);
        }
    }
}
```

## Problem 2

Problem 2 can be solved by the method below with an O(n*logn) method, but it this the best answer?

Of course not. In the solution 1, we think the problem in a positive-going way. But this time, we will think it in reverse.

If ``A[i] = u``, the number ``u`` can only appear in ``B[u+1 ... n-1]``. And ``B[i] == B[i-1]`` if there is no proper number for B[i].

So the formula is like this: ``B[i] = max(reverse(u), B[i - 1])``. And ``reverse()`` here stands for ``reverse(u) = min(i | A[i] == u)``.

However, by now, I can't find a in place algorithm for this problem. But the code below is good enough for this puzzle.

```cpp
// this code is written by @Roba
void solve(int n, int *A, int *B) {
    memset(B, -1, n * sizeof(int));
    for (int i = 0; i < n ; ++i) {
        if (i > 0) B[i] = max(B[i], B[i-1]);
        int p = A[i] + 1;
        if (p <= i) p = i + 1;
        if (p >= n) continue;
        B[p] = max(B[p], A[i]);
    }
}
```

## Thanks

thanks @Roba for [this](http://www.zhihu.com/question/25356539) answer.
