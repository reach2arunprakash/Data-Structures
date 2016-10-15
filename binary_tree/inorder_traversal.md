# Inorder Traversal

[Leetcode](https://leetcode.com/problems/binary-tree-inorder-traversal/)

題意：

Given a binary tree, return the inorder traversal of its nodes' values.

For example:
Given binary tree {1,#,2,3},
```
   1
    \
     2
    /
   3
   ```
return [1,3,2].

Note: Recursive solution is trivial, could you do it iteratively?

解題思路：



updated on 2015.12.25

```java
public class Solution {
    
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode cur = root;
        while (!stack.isEmpty() || cur != null) {
            // 先往左邊不斷的push進stack
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }
            
            cur = stack.pop();
            res.add(cur.val);
            cur = cur.right;
        }
        
        return res;
        
    }
    
    
}
```

Morris Traversal 利用Threaded BT 來達到O(1)的空間複雜度。

詳細說明請參考網友精彩的解說 [Link](http://www.cnblogs.com/AnnieKim/archive/2013/06/15/morristraversal.html)

>在Morris方法中不需要為每個節點額外分配指針指向其前驅（predecessor）和後繼節點（successor），只需要利用葉子節點中的左右空指針指向某種順序遍歷下的前驅節點或後繼節點就可以了。


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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root == null) {
            return res;
        }
        
        TreeNode cur = root;
        TreeNode prev = null;
        
        while (cur != null) {
            if (cur.left == null) {
                res.add(cur.val);
                cur = cur.right;
            } else {
                
                // find predecessor
                prev = cur.left;
                while (prev.right != null && prev.right != cur) {
                    prev = prev.right;
                }
                
                // 如果前驅節點的右孩子為空，將它的右孩子設置為當前節點。當前節點更新為當前節點的左孩子。
                if (prev.right == null) {
                    prev.right = cur;
                    cur = cur.left;
                } else {
                    // 如果前驅節點的右孩子為當前節點，將它的右孩子重新設為空（恢復樹的形狀）。輸出當前節點。當前節點更新為當前節點的右孩子。
                    prev.right = null;
                    res.add(cur.val);
                    cur = cur.right;
                }
            }
        }
        
        return res;
    }
}
```

```java
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: Inorder in ArrayList which contains node values.
     */

    //version 2 Divide and Conquer
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> res = new ArrayList<Integer>();

        if (root == null) {
            return res;
        }

        //Divide
        ArrayList<Integer> left = inorderTraversal(root.left);
        ArrayList<Integer> right = inorderTraversal(root.right);

        //Conquer
        res.addAll(left);
        res.add(root.val);
        res.addAll(right);
        return res;
    }

    //version 3 Iterative
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        // write your code here
        ArrayList<Integer> res = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode prev = null;
        TreeNode curr = root;

        if (root == null) {
            return res;
        }

        stack.push(root);
        while (!stack.isEmpty()) {
            curr = stack.peek();
            if (prev == null || prev.left == curr || prev.right == curr) {
                if (curr.left != null) {
                    stack.push(curr.left);
                } else if (curr.right != null) {
                    stack.push(curr.right);
                }
            } else if (curr.left == prev) {
                if (curr.right != null) {
                    stack.push(curr.right);
                }
            } else {
                res.add(curr.val);
                stack.pop();
            }
            prev = curr;
        }
        return res;
    }


}
```


---
###Reference
1. https://github.com/leetcoders/LeetCode/blob/master/BinaryTreeInorderTraversal.h
2. http://www.cnblogs.com/AnnieKim/archive/2013/06/15/morristraversal.html