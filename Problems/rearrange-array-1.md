[Metadata]
title: Rearrange array in alternating positive & negative items - 1
date: 2014-09-10 08:01:26 

[Tags]
difficulty: 2.5
categories: partition, implementation
source: geeksforgeeks

[Description]
Given an array of positive and negative numbers, arrange them such that the all negative numbers are on the left side of the positive numbers.

```
Example Input:
arr[] = {1, 2, 3, -4, -1, 4}

One Possible Output: 
arr[] = {-4, -1, 1, 2, 3, 4} 
```

[Solution]

Simple partition manipulation.

```cpp
void partition(vector<int>& vec)
{
    int l = 0, r = vec.size() - 1;
    while (true) {
        while (l < r && vec[l] <= 0) {
            l++;
        }
        while (l < r && vec[r] > 0) {
            r--;
        }
        if (l < r) {
            swap(vec[l], vec[r]);
        } else {
            break;
        }
    }
}
```
