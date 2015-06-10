[Metadata]
title: Kth Smallest Number in Sorted Matrix
date: 2015-06-11 01:06:01 

[Tags]
difficulty: 2.5
categories: merge sort, priority queue
source: Lintcode

[Description]
Find the kth smallest number in at row and column sorted matrix.

Example
Given k = 4 and the matrix:

```
[
  [1 ,5 ,7],
  [3 ,7 ,8],
  [4 ,8 ,9],
]
```

The answer here is 5.

[Solution]

At the very first thought, we can easily recongnize that the matrix is actually a **Young tableau**, which has a similar feature (not very similar, huh) as a binary search tree. And we try so hard to find something interesting in this matrix and finally we get nothing.

So, that why I add this "easy problem" to the problem set. Easy, but misleading.

Of course, it is a Young tableau. But here, the characteristic of the tableau is of no use. The problem is actually something like this:

> Given N sorted arrays, try to find the kth smallest element among those arrays.

So, please never imprison your mind. And try to stay calm, stay frosty, don't be misleaded by the sence of deja-vu.

```cpp
class Solution {
public:
    /**
     * @param matrix: a matrix of integers
     * @param k: an integer
     * @return: the kth smallest number in the matrix
     */
    int kthSmallest(vector<vector<int> > &matrix, int k) {
        int n = matrix.size();
        if (n == 0) {
            return -1;
        }
        int m = matrix[0].size();
        if (m == 0) {
            return -1;
        }
        
        priority_queue<
                pair<int, int>, 
                vector<pair<int, int> >,
                std::function<bool(pair<int, int>, pair<int, int>)>
        > pq(
            [&matrix]
            (pair<int, int> pa, pair<int, int> pb) {
                return matrix[pa.first][pa.second] > matrix[pb.first][pb.second];
            }
        );
        
        for (int i = 0; i < n; i++) {
            pq.push({i, 0});
        }
        
        for (int i = 0; i < k - 1; i++) {
            auto p = pq.top();
            pq.pop();
            
            int y = p.first;
            int x = p.second;
            
            if (x + 1 < m) {
                pq.push({y, x + 1});
            }
        }
        return matrix[pq.top().first][pq.top().second];
    }
};
```
