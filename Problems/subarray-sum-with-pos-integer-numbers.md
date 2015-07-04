[Metadata]
title: Subarray Sum with Positive Integer Numbers
date: 2015-07-04 23:24:17 

[Tags]
difficulty: 4
categories: two-pointers
source: lintcode

[Description]

Given an positive integer array, find a subarray where the sum of numbers is between two given interval. Your code should return the number of possible answer.

For example, Given [1,2,3,4] and interval = [1,3], return 4. The possible answers are:

```
[0, 0]
[0, 1]
[1, 1]
[2, 2]
```

[Solution]

It's Okay to use `std::map<int, int>` to deal with the problem, the time complexity is `O(n * logn)` and space complexity is `O(n)`.

However, there is a better way to deal with it. The crux of problem is, the number in this array is all positive numbers. So, for the interval sum, `sum(A[0...i])`, it's obviously an increasing sequence. That is an important property that we can take advantage of.

For example, if `sum(A[p...q])` is greater than `K`, and it's obvious that `sum(A[p...q+t])` is greater than `K`. See, it's similar to the basic "Two Sum" problem. Of course, the solution is quite same.

Back to this problem, we want a subarray sum which is in the range of `[st, end]`. That means we have to find out two numbers of the "subarray sum":

1. The number of subarrays whose sum is greater or equal to `st`
2. The number of subarrays whose sum is strictly less than `end`

In the end, we substract the two numbers to get the final answer.

In the code, we can take advantage of the `lambda expression` to avoid writing the same code. DRY(Don't Repeat Yourself) is the virtue of a real programmer.

```cpp
class Solution {
public:
    /**
     * @param A an integer array
     * @param start an integer
     * @param end an integer
     * @return the number of possible answer
     */
    int subarraySumII(vector<int>& A, int start, int end) {
        int a = solve(A, [start](int x) {
            return x >= start;
        });
        int b = solve(A, [end](int x) {
            return x > end;
        });
        return a - b;
    }
private:
    int solve(const vector<int>& A, function<int(int)> fun) {
        int n = A.size();
        int sum = A[0];
        int p = -1;
        int q = 0;
        int ans = 0;
        
        while (p < n && q < n) {
            sum -= p >= 0? A[p]: 0;
            while (q < n && !fun(sum)) {
                q++;
                sum += A[q];
            }
            ans += n - q;
            p++;
        }
        return ans;
    }
};
```
