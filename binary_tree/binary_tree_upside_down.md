# Binary Tree Upside Down

[Leetcode](https://leetcode.com/problems/binary-tree-upside-down/)


題意：

Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.

For example:
Given a binary tree {1,2,3,4,5},
```
    1
   / \
  2   3
 / \
4   5
```
return the root of the binary tree [4,5,2,#,#,3,1].
```
   4
  / \
 5   2
    / \
   3   1  
   ```


解題思路：

目前還是搞不太清楚怎弄，先記錄下來。

[分析]
起始對於每一個節點，相應的操作為：
- p.left = parent.right;
- p.right = parent;
- 
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
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        TreeNode p = root;
        TreeNode parent = null;
        TreeNode parentRight = null;
        while (p != null) {
            TreeNode left = p.left;
            p.left = parentRight;
            parentRight = p.right;
            p.right = parent;
            parent = p;
            p = left;
        }
        return parent;
    }
}
```

---
###Reference
1. http://www.danielbit.com/blog/puzzle/leetcode/leetcode-binary-tree-upside-down
2. http://blog.csdn.net/whuwangyi/article/details/43186045