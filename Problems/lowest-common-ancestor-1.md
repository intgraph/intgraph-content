[Metadata]
title: Lowest Common Ancestor - 1
date: 2015-06-25 00:14:46 

[Tags]
difficulty: 3
categories: binary tree, lca, tree
source: Lintcode

[Description]

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

The node of the binary search tree is represent in this way:

```cpp
class TreeNode {
public:
    int val;
    TreeNode *left, *right;
    TreeNode(int val) {
        this->val = val;
        this->left = this->right = NULL;
    }
};
```

[Solution]

There are multiple scenarios for one single node in the binary tree during we trying to find the LCA of the two nodes.

* Scenario 1: node N is equal to A and B      
This is the simplest one, because node A (or node B, they are the same) is the LCA itself.
* Scenario 2: node N is equal to A, as well as the ancestor of B     
Then, node N is the LCA of node A and node B
* Scenario 3: node N is equal to A, but is not the ancestor of B     
The LCA of A and B must be one of the ancestor of N 
* Scenario 4: there is no node A and node B among node N and its children     
The LCA of A and B should be one of the ancestor of N  

Now, we can write our code like this:

```cpp
class Solution {
public:
    TreeNode *lowestCommonAncestor(TreeNode *root, TreeNode *A, TreeNode *B) {
        ans = nullptr;
        if (A == B) {
            return A;
        }
        do_lowestCommonAncestor(root, A, B);
        return ans;
    }
private:
    TreeNode *do_lowestCommonAncestor(TreeNode* root, TreeNode *A, TreeNode *B) {
        if (root == nullptr) {
            return nullptr;
        }
        
        auto* pl = do_lowestCommonAncestor(root->left, A, B);
        auto* pr = do_lowestCommonAncestor(root->right, A, B);
        
        
        if (root == A || root == B) {
            if (pl || pr) {
                ans = root;
            }
            return root;
        }
        
        if (pl == A && pr == B) {
            ans = root;
            return nullptr;
        }
        if (pl == B && pr == A) {
            ans = root;
            return nullptr;
        }
        
        if (pl) return pl;
        if (pr) return pr;
        
        return nullptr;
    }
private:
    TreeNode* ans;
};
```

And there is a cooler version, but they are actually the same.

```cpp
class Solution {
public:
    TreeNode *lowestCommonAncestor(TreeNode *root, TreeNode *A, TreeNode *B) {
        if (root == nullptr) {
            return nullptr;
        }
        
        if (root == A || root == B) {
            return root;
        }
        
        auto* pl = lowestCommonAncestor(root->left, A, B);
        auto* pr = lowestCommonAncestor(root->right, A, B);
        if (pl && pr) {
            return root;
        }
        return pl? pl: pr;
    }
};
```
