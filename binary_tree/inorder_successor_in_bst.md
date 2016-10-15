# Inorder Successor in BST

[Leetcode](https://leetcode.com/problems/inorder-successor-in-bst/)


題意：

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

Note: If the given node has no in-order successor in the tree, return null.


解題思路：

是要找bst中某個節點的中序後繼者，如果是往左走，則當下的root有可能是該節點的後繼者，需紀錄下來，若是往右走，則後繼者不變，程式碼如下：


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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if (root == null) {
            return root;
        }
        
        TreeNode succ = null;
        while (root != null) {
            if (p.val < root.val) {
                succ = root;
                root = root.left;
            } else {
                root = root.right;
            }
        }
        
        return succ;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/61105/java-python-solution-o-h-time-and-o-1-space-iterative
2. https://leetcode.com/discuss/59787/share-my-java-recursive-solution