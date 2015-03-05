[Metadata]
title: Heapify
date: 2015-03-06 00:39:41 

[Tags]
difficulty: 3
categories: heap
source: Lintcode

[Description]

Given an integer array, heapify it into a min-heap array.
For a heap array A, A[0] is the root of heap, and for each A[i], A[i * 2 + 1] is the left child of A[i] and A[i * 2 + 2] is the right child of A[i].

Problem Link: http://lintcode.com/en/problem/heapify/

```
Sample Input
[3,2,1,4,5]

Sample Output
[1,2,3,4,5] 

p.s. Any solution which is a min-heap array is acceptable.
```

[Solution]

The basic solution is well known. As an array with only one element is of course a min-heap array. And then we add one number at one time, and keep the new array as a min-heap.

The time complexity of time algorithm is `O(logN * N)` . For `N = 2^k`, it needs `sum(2^i * i)` steps to heapify the array.

```cpp
// O(n * logn) time complexity
class Solution {
public:
    /**
     * @param A: Given an integer array
     * @return: void
     */
    void heapify(vector<int> &A) {
        int n = A.size();
        for (int i = 0; i < n; i++) {
            int p = i;
            while (p && A[parent(p)] > A[p]) {
                swap(A[parent(p)], A[p]);
                p = parent(p);
            }
        }
    }
private:
    int parent(int x) {
        return (x - 1) / 2;
    }
};
```

But there is another way to heapify the array, with only O(N) time complexity. This algorithm is not a fancy one, but it could really make sense.

As we describe above, we build the min-heap from the top to bottom. If we build the heap from bottom to top, we could make it faster.

To build a min-heap from bottom to top, it will take `sum(2^i * (k - i))` step to build that heap. And the final time complexity is `O(N)`.

```cpp
// O(n) time complexity
class Solution {
public:
    /**
     * @param A: Given an integer array
     * @return: void
     */
    void heapify(vector<int> &A) {
        int n = A.size();
        
        for(int i = flp2(n) - 1; i >= 0; i--) {
            down(A, i);
        }
    }
private:
    int down(vector<int> &A, int p) {
        int l = left(p);
        int r = right(p);
        int n = A.size();
        
        if (l < n && A[l] < A[p]) {
            swap(A[l], A[p]);
            down(A, l);
        }
        
        if (r < n && A[r] < A[p]) {
            swap(A[r], A[p]);
            down(A, r);
        }
    }            
        
    int left(int x) {
        return (x << 1) + 1;
    }
    int right(int x) {
        return (x << 1) + 2;
    }
    int flp2(int x) {
        x |= (x >> 1);
        x |= (x >> 2);
        x |= (x >> 4);
        x |= (x >> 8);
        x |= (x >> 16);
        return x - (x >> 1);
    }
};
```
