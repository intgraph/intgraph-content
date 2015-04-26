[Metadata]
title: Binary Tree Inorder Traversal 
date:  2015-04-26 20:11:01 

[Tags]
difficulty: 3.5
categories: tree
source: Leetcode, tencent

[Description]

Given a binary tree, return the inorder traversal of its nodes' values.

For example:

![Alt text](data:image,local://1430039359672)

return `[1,3,2]`.

You may finish the job with different ways:

* Recursive solution
* Iterative solution with a stack
* Iterative solution without a stack

[Solution]

### Recursive solution

Recursive soluitin is trivial, and it's the classical algorithm among the novices to understand to recursive.

I just paste the code here.

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        res = new ArrayList<Integer>();
        do_inorderTraversal(root);
        return res;
    }
    
    private void do_inorderTraversal(TreeNode root) {
        if (root == null) {
            return;
        }
        if (root.left != null) {
            do_inorderTraversal(root.left);
        }
        res.add(root.val);
        if (root.right != null) {
            do_inorderTraversal(root.right);
        }
    }
    private List<Integer> res;
}
```

### Iterative solution with a stack

The order of "inorder traversal" is: left child(ren) first, current node second, right child(ren) last.

So, in the stack, we store the "current nodes", and using an extra pointer which points to explore the "left child(ren)" of current node.

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Stack<TreeNode> st = new Stack<TreeNode>();
        
        if (root == null) {
            return res;
        }

        TreeNode p = root;
        
        while (true) {
            if (p != null) {
                st.push(p);
                p = p.left;
            } else if (!st.empty()) {
                p = st.pop();
                res.add(p.val);
                p = p.right;
            } else {
                break;
            }
        }
        return res;
    }
}
```

### Iterative solution without a stack

Yeah, this one is difficult, and it's quite different from the methods above. 

For every nodes in the tree, there are two conditions:

* a node has a traversed left child(ren)
* a node not has a traversed left child(ren)

And how can we tell the difference from these two conditions without using extra memory space?

Okay, let's just put that question aside, and think about a new one.

As we don't use extra memory space, how can we know which node is next if we are at the leaf nodes of the tree.

The [Morris in-order traversal using treading][1] is the solution for the two problems above.

The traversal method is like this:

1. if the left child of current node is null, add this node to the result list, and move current pointer to the right child of the node
2. if the left child of current node is not null, we will try to find the previous node in the left sub tree
    * move the pointer to the right most node of the left sub tree of current node
    * if the right child of this node is null, set its right child to current node
    * if the right child of this node is current node, it shows that we have already traversed this sub tree, add current node to the result list, clear the temporary pointer of the node, and set current pointer to the right child of current node.

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        TreeNode cur = root;
        TreeNode prev = null;
        
        List<Integer> res = new ArrayList<Integer>();
        
        while (cur != null) {
            if (cur.left == null) {
                res.add(cur.val);
                cur = cur.right;
                continue;
            }
            
            prev = cur.left;
            
            while (prev.right != null && prev.right != cur) {
                prev = prev.right;
            }
            
            if (prev.right == null) {
                prev.right = cur;
                cur = cur.left;
            } else {
                prev.right = null;
                res.add(cur.val);
                cur = cur.right;
            }
        }
        return res;
    }
}
```

[1]: http://en.wikipedia.org/wiki/Tree_traversal#Morris_in-order_traversal_using_threading
