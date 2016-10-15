# Invert Binary Tree

[Leetcode](https://leetcode.com/problems/invert-binary-tree/)

題意：

Invert a binary tree.
```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```
to
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

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
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        
        TreeNode left = invertTree(root.right);
        TreeNode right = invertTree(root.left);
        
        root.left = left;
        root.right = right;
        
        return root;
    }
}
```

非遞迴法： 用level order方式去作。

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
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        
        while(!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = q.poll();
                
                if (cur.left != null) {
                    q.offer(cur.left);
                }
                
                if (cur.right != null) {
                    q.offer(cur.right);
                }
                
                TreeNode temp = cur.left;
                cur.left = cur.right;
                cur.right = temp;
            }
        }
        
        return root;
    }
}
```