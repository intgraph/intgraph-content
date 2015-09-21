[Metadata]
title: Lower-bound & Upper-bound in a Binary Search Tree
date: 2015-08-10 22:35:35

[Tags]
difficulty: 3
categories: implementation
source: Facebook

[Description]

Given a binary search tree, which contains only integers, and may contain duplicated values. And we only know that the tree is a legal binary search tree, that is, `left_child <= root <= right_child`.

Your mission is to find the lower-bound and upper-bound of a given number `key`.

The behavior of the functions are similar to `std::lower_bound` and `std::upper_bound`.

For example,

```
/*
                      3
                    /   \
                    2    4
                   / \    \
lower_bound(2) ->  2 2    8 <- upper_bound(4)
*/
```

The `lower_bound(2)` function should return the pointer to the lowest node with value `2`. And `upper_bound(4)` should return the lowest node with value `8`.

[Solution]

Simple implementation problem.

```cpp
class Solution {
public:
    TreeNode* upper_bound(TreeNode* root, int value) {
        ub = nullptr;
        do_upper_bound(root, value);
        return ub;
    }

    TreeNode* lower_bound(TreeNode* root, int value) {
        lb = nullptr;
        do_lower_bound(root, value);
        return lb;
    }

private:
    void do_upper_bound(TreeNode* root, int value) {
        if (root == nullptr) {
            return;
        }
        if (root->value > value) {
            ub = root;
            do_upper_bound(root->left, value);
        } else {
            do_upper_bound(root->right, value);
        }
    }

    void do_lower_bound(TreeNode* root, int value) {
        if (root == nullptr) {
            return;
        }
        if (root->value >= value) {
            lb = root;
            do_lower_bound(root->left, value);
        } else {
            do_lower_bound(root->right, value);
        }
    }
private:
    TreeNode* ub;
    TreeNode* lb;
};
```
