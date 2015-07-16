[Metadata]
title: Print All Paths from Root to Leaves
date: 2015-07-17 00:36:00

[Tags]
difficulty: 3.5
categories: binary tree
source: Facebook

[Description]

Given a binary tree. Your mission is to print all the paths from root to leaves.

Follow up:

It's easy to solve the problem recursively. But can you deal with it iteratively.

```

      A
     / \
   B     C
   \     /
    D   E
```

[Solution]

The DFS solution is simple.

```cpp
struct TreeNode {
    char c;
    TreeNode* left;
    TreeNode* right;

    TreeNode(char i_c): c(i_c), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<string> path_traversal(TreeNode* root) {
        res.clear();
        do_traversal(root);
        return res;
    }

    void do_traversal(TreeNode* root) {
        if (root == nullptr) {
            return;
        }
        path += root->c;
        if (!root->left && !root->right) {
            res.push_back(path);
        }
        do_traversal(root->left);
        do_traversal(root->right);
        path.pop_back();
    }
private:
    string path;
    vector<string> res;
};
```

The interative solution is quite tricky. Because we don't quite use this kind of algorithm in our daily job. But sometimes you have to remember some classic algorithm to cope with the interviewers. :)

```cpp
class Solution {
public:
    vector<string> path_traversal(TreeNode* root) {
        res.clear();
        do_traversal(root);
        return res;
    }

    void do_traversal(TreeNode* ptr) {
        while (ptr || !st.empty()) {
            if (ptr) {
                st.push_back(ptr);
                path += ptr->c;
                if (!ptr->left && !ptr->right) {
                    res.push_back(path);
                }
                ptr = ptr->left;
            } else {
                ptr = *st.rbegin();
                ptr = ptr->right;

                if (!ptr) {
                    ptr = *st.rbegin();
                    while (!st.empty()) {
                        st.pop_back();
                        path.pop_back();
                        if (st.empty()) {
                            ptr = nullptr;
                            break;
                        }
                        auto cur = *st.rbegin();
                        if (cur->right == ptr) {
                            ptr = cur;
                        } else {
                            ptr = cur->right;
                            break;
                        }
                        ptr = cur;
                    }
                }
            }
        }
    }
private:
    string path;
    vector<TreeNode*> st;
    vector<string> res;
};
```

The code below is similar to the binary tree traversal with a stack，but more complicated. Because you have to print the path.

And here is another solution, which is a more simple iterative solution -- BFS. I think it's much better than the previous one. This is the "security flaw" that we can take advantage of.

```cpp
class Solution {
public:
    vector<string> path_traversal(TreeNode* root) {
        res.clear();
        do_traversal(root);
        return res;
    }

    void do_traversal(TreeNode* ptr) {
        queue<QueueNode> q;
        q.push({"", ptr});

        while (!q.empty()) {
            auto cur = q.front();
            q.pop();
            if (cur.ptr->left) {
                auto nq = QueueNode({
                    cur.path + cur.ptr->c,
                    cur.ptr->left
                });
                q.push(nq);
            }
            if (cur.ptr->right) {
                auto nq = QueueNode({
                    cur.path + cur.ptr->c,
                    cur.ptr->right
                });
                q.push(nq);
            }
            if (!cur.ptr->left && !cur.ptr->right) {
                res.push_back(cur.path + cur.ptr->c);
            }
        }
    }
private:
    struct QueueNode {
        string path;
        TreeNode* ptr;
    };
    vector<string> res;
};
```
