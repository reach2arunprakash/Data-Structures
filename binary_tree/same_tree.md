# Same Tree

[Leetcode](https://leetcode.com/problems/same-tree/)


題意：

Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.


解題思路：



遞迴法：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        } else if (p == null || q == null) {
            return false;
        }
        
        if (p.val != q.val) {
            return false;
        } else {
            return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
        }
    }
}
```

非遞迴法：用dfs加stack來作

```java
public boolean isSameTree(TreeNode p, TreeNode q) {
    // iteration method
    if (p == null && q == null) return true;
    if (p == null && q != null || p != null && q == null) return false;
    Stack<TreeNode> stackP = new Stack<>();
    Stack<TreeNode> stackQ = new Stack<>();
    stackP.add(p);
    stackQ.add(q);
    while (!stackP.isEmpty() && !stackQ.isEmpty()) {
        TreeNode tmpP = stackP.pop();
        TreeNode tmpQ = stackQ.pop();
        if (tmpP.val != tmpQ.val) return false;
        if (tmpP.left != null && tmpQ.left != null) {
            stackP.push(tmpP.left);
            stackQ.push(tmpQ.left);
        } else if (tmpP.left == null && tmpQ.left == null) {
        } else {
            return false;
        }
        if (tmpP.right != null && tmpQ.right != null) {
            stackP.push(tmpP.right);
            stackQ.push(tmpQ.right);
        } else if (tmpP.right == null && tmpQ.right == null) {
        } else {
            return false;
        }
    }
    if (!stackP.isEmpty() || !stackQ.isEmpty()) return false;
    return true;
}
```