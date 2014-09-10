[Metadata]
title: Rearrange array in alternating positive & negative items
date: 2014-09-10 08:58:13 

[Tags]
difficulty: 3.5
categories: implementation
source: geeksforgeeks, amazon

[Description]
Given an array of positive and negative numbers, arrange them in an alternate fashion such that every positive number is followed by negative and vice-versa maintaining the order of appearance.

Number of positive and negative numbers need not be equal. If there are more positive numbers they appear at the end of the array. If there are more negative numbers, they too appear in the end of the array.

Example:

```
Input:  arr[] = {1, 2, 3, -4, -1, 4}
Output: arr[] = {-4, 1, -1, 2, 3, 4}

Input:  arr[] = {-5, -2, 5, 2, 4, 7, 1, 8, 0, -8}
output: arr[] = {-5, 5, -2, 2, -8, 4, 7, 1, 8, 0} 
```

Please deal with it with O(n) time complexity and O(1) extra memory space.

[Solution]

The problem is not easy, and may easily make some tiny mistakes and screw up.

The solution below is not an elegant one but it's correct (tested by 1000 random cases) and easy to understand.

If you have a better answer, please tell me by sending a pull request on intgraph-content repo at github.

```cpp
void rearrange(vector<int>& vec)
{
    int n = vec.size();
    int neg = 0, pos = 1;
    while (neg < n && pos < n) {
        while (neg < n && vec[neg] < 0)  neg += 2;
        while (pos < n && vec[pos] >= 0) pos += 2;
        
        if (neg < n && pos < n) {
            swap(vec[neg], vec[pos]);
        } else if (neg < n) {
            int l = neg, r = n % 2 == 0? n: n - 1;
            while (l < r) {
                if (vec[r] < 0) {
                    swap(vec[l], vec[r]);
                    l += 2;
                } else {
                    r -= 2;
                }
            }
            break;
        } else if (pos < n) {
            int l = pos, r = n % 2 == 0? n - 1: n;
            while (l < r) {
                if (vec[r] >= 0) {
                    swap(vec[l], vec[r]);
                    l += 2;
                } else {
                    r -= 2;
                }
            }
            break;
        }
    }
}
```
