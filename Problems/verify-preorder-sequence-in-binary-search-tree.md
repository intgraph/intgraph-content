[Metadata]
title: Verify Preorder Sequence in Binary Search Tree
date: 2015-08-15 14:18:29

[Tags]
difficulty: 2.5
categories:  tree
source: Leetcode

[Description]

You have an array of preorder traversal of Binary Search Tree ( BST). Your program should verify whether it is a correct sequence or not.

Input: Array of Integer [ Pre-order BST ]      
Output: String “Yes” or “No”

```
>> Sample Input
    1 2 3
    2 1 3
    3 2 1 5 4 6
    1 3 4 2
    3 4 5 1 2

>> Sample Output
    YES
    YES
    YES
    NO
    NO
```

[Solution]

It's known to us all that this problem has an `O(n^2)` solution.

```cpp
class Solution {
public:
    bool verifyPreorder(vector<int>& preorder) {
        return do_verify_preorder(preorder.begin(), preorder.end());
    }
private:
    bool do_verify_preorder(vector<int>::iterator st, vector<int>::iterator end) {
        if (st == end) {
            return true;
        }

        int u = *st;

        auto last_le = st;
        auto first_gt = end;
        for (auto iter = st + 1; iter != end; ++iter) {
            if (*iter <= u) {
                last_le = iter;
            } else if (first_gt == end) {
                first_gt = iter;
            }
        }

        if (last_le + 1 == first_gt) {
            return do_verify_preorder(st + 1, first_gt) && do_verify_preorder(first_gt, end);
        }
        return false;
    }
}
```

The main idea for this solution is to split the array to the left & right part of the tree. And then check if there are invalid elements in the both subtree.

This is correct, but not optimized. And here is another idea that deferring the check of the subtree until the leaves node.

```cpp
class Solution {
public:
    bool verifyPreorder(vector<int>& preorder) {
        return do_verify_preorder(
            preorder.begin(),
            preorder.end(),
            numeric_limits<int>::min(),
            numeric_limits<int>::max()
        );
    }
private:
    bool do_verify_preorder(
            vector<int>::iterator st,
            vector<int>::iterator end,
            int mini,
            int maxi) {
        if (st + 1 == end) {
            return mini <= st && st <= end;
        }

        int u = *st;
        auto mid = find_if(st, end, [&](int value) {
            return value > u;
        });

        return do_verify_preorder(st + 1, mid, mini, *mid) && \
                do_verify_preorder(mid, end, *mid, maxi);
    }
}
```
