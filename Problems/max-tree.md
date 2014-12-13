[Metadata]
title: Max Tree
date: 2014-12-13 20:57:41 

[Tags]
difficulty: 4
categories: tree, stack
source: Lintcode

[Description]
Given an integer array with no duplicates. A max tree building on this array is defined as follow:

* The root is the maximum number in the array
* The left subtree and right subtree are the max trees of the subarray divided by the root number.
* The sequence of the in-order traversal of this max tree will be equal to the given array

Please construct the max tree by the given array.

For example, the max tree for [2, 5, 6, 0, 3, 1] is:

```
              6

            /    \

          5       3

        /       /   \

      2       0       1
```

[Solution]

To build a max tree is similar to build a heap (a.k.a priority queue). But usually, building a heap takes O(n * logn) time. We need a faster method to handle a max tree.

Suppose we have already got a max tree with size k. We will call it MaxTree(k). When adding a new node, Node(k + 1), into the tree, we may face these conditions:

* the value of Node(k + 1) is less than Node(k)    
In this scenario, the new node will be the leftmost node of the right sub-tree of Node(k). As we one by one insert the node to the max tree, so the right sub-tree of Node(k) must be empty.

* the value of Node(k + 1) is greater (or equal) to Node(k)      
In this scenario, we have to find a parent node of Node(k) which is greater than Node(k + 1), and make it as the left sub-tree of Node(k + 1). If that node is not exist, Node(k) will be the root node.

So we use a stack to storage the parent nodes of Node(k). We insert one node at one time. And all of the nodes will be pushed to the stack for only once. So the time complexity of the method is O(N).

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param A: Given an integer array with no duplicates.
     * @return: The root of max tree.
     */
    public TreeNode maxTree(int[] A) {
        int n = A.length;
        TreeNode root = null;
        for (int i = 0; i < n; i++) {
            root = insert(root, A[i]);
        }
        return root;
    }
    
    private TreeNode insert(TreeNode root, int v) {
        TreeNode ptr = null;
        TreeNode now = new TreeNode(v);
        
        while (!stack.empty()) {
            ptr = stack.peek();
            if (ptr.val > v) {
                now.left = ptr.right;
                ptr.right = now;
                stack.push(now);
                return root;
            }
            stack.pop();
        }
        now.left = ptr;
        stack.push(now);
        return now;
    }
    
    private Stack<TreeNode> stack;
}


```
