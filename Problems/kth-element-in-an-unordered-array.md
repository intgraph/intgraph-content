[Metadata]
title: Get the kth element in an unordered array
date: 2014-11-02 01:11:39 

[Tags]
difficulty: 3.5
categories: partition, quick-sort
source: mitbbs, facebook, baidu, vintage

[Description]

Given an unordered array which contain K numbers. Find the largest K numbers with the method which runs in O(n) time complexity.

[Solution]

This is a vintage problem. I'll show you the code here.

```cpp
// @author  wizmann
// @tested  manual & random data
int kth_element(int vec[], int n, int k) {
    int pivot = vec[0];
    int l = 0, r = n - 1;
    while (l <= r) {
        while (l < n && vec[l] <= pivot) {
            l++;
        }
        while (r >= 0 && vec[r] > pivot) {
            r--;
        }
        if (l <= r) {
            swap(vec[l], vec[r]);
        }
    }
    if (l == k) {
        return pivot;
    } else if (l > k) {
        swap(vec[0], vec[l - 1]); // <- Crucial
        return kth_element(vec, n - 1, k);
    } else {
        return kth_element(vec + l, n - l, k - l);
    }
}
```

But how to prove this algorithm will run in O(n) time complexity?

Assuming every "partition" will separate this array into two equal length subarray. It's easy to know that the kth element whether in the left side or the right side.

So the first "partition" manipulation will deal with n elements, and the second "partition" will deal with n/2 elements.

```mathjax
\begin{equation}
\begin{split}
T &= \frac{1 - q^n}{1 - q} * n \quad(q = \frac{1}{2}) \\\\
  &= \frac{1 - \frac{1}{2}^n}{\frac{1}{2}} * n \\\\
  &= 2n 
\end{split}
\end{equation}
```

So the time complexity here we prove is O(N). However, the worst case will lead to an algorithm runs in O(N^2) time.
