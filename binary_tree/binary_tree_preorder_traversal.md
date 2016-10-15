# Binary Tree Preorder Traversal

[Leetcode]https://leetcode.com/problems/binary-tree-preorder-traversal/

題意：

Given a binary tree, return the preorder traversal of its nodes' values.

For example:
Given binary tree {1,#,2,3},
```
   1
    \
     2
    /
   3
   ```
return [1,2,3].

Note: Recursive solution is trivial, could you do it iteratively?


解題思路：

遞迴法：

Stack法：

```java
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        
        if (root == null) {
            return res;
        }
        
        Stack<TreeNode> s = new Stack<>();
        s.push(root);
        while (!s.isEmpty()) {
            TreeNode cur = s.pop();
            res.add(cur.val);
            if (cur.right != null) {
                s.push(cur.right);
            }
            if (cur.left != null) {
                s.push(cur.left);
            }
        }
        
        return res;
    }
}
```


Morris法：此法不需要耗用額外的空間，利用類似Thread Binary Tree的方法，利用空的指針指向前序後繼者。

網友提供非常詳細的解說 [Link](http://www.cnblogs.com/AnnieKim/archive/2013/06/15/morristraversal.html)


```java
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        
        TreeNode cur = root;
        TreeNode prev = null;
        while (cur != null) {
            if (cur.left == null) {
                res.add(cur.val);
                cur = cur.right;
            } else {
                prev = cur.left;
                while (prev.right != null && prev.right != cur) {
                    prev = prev.right;
                }
                
                if (prev.right == null) {
                    res.add(cur.val);
                    prev.right = cur;
                    cur = cur.left;
                } else {
                    prev.right = null;
                    cur = cur.right;
                }
            }
        }
        
        return res;
    }
}
```

---
###Reference
1. http://www.cnblogs.com/AnnieKim/archive/2013/06/15/morristraversal.html