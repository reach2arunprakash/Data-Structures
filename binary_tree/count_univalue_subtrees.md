# Count Univalue Subtrees

[Leetcode](https://leetcode.com/problems/count-univalue-subtrees/)

題意：

找出所有subtree值全一樣的個數。


Given a binary tree, count the number of uni-value subtrees.

A Uni-value subtree means all nodes of the subtree have the same value.

For example:
Given binary tree,
```
              5
             / \
            1   5
           / \   \
          5   5   5
          ```
return 4.


解題思路：

使用遞迴去作，不斷的把root的val傳下去作比較，一但比到錯的，則返回false。

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
    public int countUnivalSubtrees(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int[] counter = new int[1];
        count(root, counter, root.val);
        return counter[0];
    }

    private boolean count(TreeNode root, int[] counter, int val) {
        if (root == null) {
            return true;
        }
        boolean left = count(root.left, counter, root.val);
        boolean right = count(root.right, counter, root.val);

        if (left && right) {
            counter[0]++;
        }

        return left && right && root.val == val;
    }

}
```
---
###Reference
1. https://leetcode.com/discuss/55213/my-ac-java-code