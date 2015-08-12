[Metadata]
title: Equal Range in a Binary Search Tree
date: 2015-08-12 22:59:26

[Tags]
difficulty: 3.5
categories: tree, binary search tree
source: facebook

[Description]

Given a binary search tree, and a range `[l, r]`. Your mission is to find out all the nodes whose value is right in the range, inclusively.

For example, in this tree

```
/*
   3
 /   \
 2    4
/ \    \
2 2     8
*/
```

There are 5 nodes in the range `[2, 4]`, that is, `[2, 2, 2, 3, 4]`.

Try to solve this problem iteratively and recursively. :)

[Solution]

It's a "basic" problem for the bianry search tree manipulation. But it's not that easy to code. Please try your best to avoid the potential problem.

That's all.

```cpp
class Solution_Iterative {
public:
    vector<int> range(TreeNode* root, int l, int r) {
        res.clear();
        st = stack<TreeNode*>();
        lb = nullptr;
        lower_bound(root, l);
        do_range(l, r);
        return res;
    }
private:
    void lower_bound(TreeNode* root, int val) {
        if (root == nullptr) {
            return;
        }
        if (val <= root->value) {
            st.push(root);
            lb = root;
            lower_bound(root->left, val);
        } else {
            lower_bound(root->right, val);
        }
    }

    void do_range(int l, int r) {
        if (lb == nullptr) {
            return;
        }

        TreeNode* ptr = lb->right;

        if (lb->value <= r) {
            res.push_back(lb->value);
        }
        st.pop();
        while (ptr || !st.empty()) {
            if (ptr) {
                st.push(ptr);
                ptr = ptr->left;
            } else {
                ptr = st.top();
                st.pop();
                if (ptr->value > r) {
                    return;
                }
                res.push_back(ptr->value);
                ptr = ptr->right;
            }
        }
    }
private:
    TreeNode* lb;
    stack<TreeNode*> st;
    vector<int> res;
};
```

```cpp
class Solution_Recursive {
public:
    vector<int> range(TreeNode* root, int l, int r) {
        res.clear();
        do_range(root, l, r);
        return res;
    }
private:
    void do_range(TreeNode* root, int l, int r) {
        if (root == nullptr) {
            return;
        }
        if (l <= root->value) {
            do_range(root->left, l, r);
        }
        if (l <= root->value && root->value <= r) {
            res.push_back(root->value);
        }
        if (root->value <= r) {
            do_range(root->right, l, r);
        }
    }
private:
    vector<int> res;
};
```
