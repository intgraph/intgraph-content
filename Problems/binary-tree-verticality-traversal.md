[Metadata]
title: Binary Tree Verticality Traversal
date:  2015-04-11 18:44:08 

[Tags]
difficulty: 3
categories: tree
source: facebook, geeksforgeeks

[Description]

To understand what's same vertical line, we need to define horizontal distances first. If two nodes have the same Horizontal Distance (HD), then they are on same vertical line.

The idea of HD is simple. HD for root is 0, a right edge (edge connecting to right subtree) is considered as +1 horizontal distance and a left edge is considered as -1 horizontal distance. 

![Alt text](http://wizmann-pic.qiniudn.com/fe9005fb7f3383b78679a06c590d3c5e)

Now given a binary tree, please print the path of verticality traversal of the tree.

![Alt text](http://wizmann-pic.qiniudn.com/f42894ec3263b013c017f8f519648bb4)

The output of print this tree vertically will be:
```
4
2
1 5 6
3 8
7
9 
```

[Solution]

The most difficult part of this problem is -- the meaning of it.

After you understand what it really wants. It not hard at all, indeed.

As you don't know the "width" of the tree. You have to use a dynamic data structure to store the nodes on each line. In my code, I use `list<vector<int> >`, but I think `unordered_map<int, vector<int> >` or `map<int, vector<int> >` will be more concise.

(I copy the skeleton of the code from [here][1], because I'm too lazy to build a tree manually.)

```cpp
#include <iostream>
#include <list>
#include <vector>
#include <algorithm>

using namespace std;
 
struct TreeNode
{
    int value;
    TreeNode *left, *right;
    
    TreeNode(int v): value(v), left(NULL), right(NULL) {}
};

list<vector<int> > ll;

void do_traversal(
        const TreeNode* root,
        int hd,
        list<vector<int> >::iterator iter) {
    if (!root) {
        return;
    }
    iter->push_back(root->value);
    
    if (root->left) {
        auto left_iter = iter;
        if (left_iter == ll.begin()) {
            ll.emplace_front();
        }
        --left_iter;
        
        do_traversal(root->left, hd - 1, left_iter);
    }
    
    if (root->right) {
        auto right_iter = iter;
        if (++right_iter == ll.end()) {
            ll.emplace_back();
        }
        right_iter = iter;
        ++right_iter;
        do_traversal(root->right, hd + 1, right_iter);
    }
}

void get_res() {
    for (const auto& vec: ll) {
        for (const auto& i: vec) {
            printf("%d ", i);
        }
        puts("");
    }
}

void verticalOrder(const TreeNode* root) {
    ll.clear();
    ll.emplace_back();
    do_traversal(root, 0, ll.begin());
    get_res();
}

int main()
{
    TreeNode *root = new TreeNode(1);
    
    root->left  = new TreeNode(2);
    root->right = new TreeNode(3);
    
    root->left->left  = new TreeNode(4);
    root->left->right = new TreeNode(5);
    
    root->right->left  = new TreeNode(6);
    root->right->right = new TreeNode(7);
    
    root->right->left->right  = new TreeNode(8);
    root->right->right->right = new TreeNode(9);
 
    cout << "Vertical order traversal is \n";
    verticalOrder(root);
 
    return 0;
}
```

[1]: http://www.geeksforgeeks.org/print-binary-tree-vertical-order/
