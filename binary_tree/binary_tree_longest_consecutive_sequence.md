# Binary Tree Longest Consecutive Sequence

[Leetcode](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/)

題意：

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

For example,
```
   1
    \
     3
    / \
   2   4
        \
         5
```
Longest consecutive sequence path is ```3-4-5```, so return 3.
```
   2
    \
     3
    / 
   2    
  / 
 1
 ```
Longest consecutive sequence path is ```2-3```,not```3-2-1```, so return 2.


解題思路：

updated 2016.1.3

非遞迴解法：使用一個stack加上一個hashmap來紀錄該treenode最長嚴格連續遞增序列的長度為多少。


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
    public int longestConsecutive(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int max = 1;
        Stack<TreeNode> stack = new Stack<>();
        HashMap<TreeNode, Integer> map = new HashMap<>();
        stack.push(root);
        map.put(root, 1);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            int left = (cur.left != null && (cur.left.val - 1) == cur.val) ? map.get(cur) + 1 : 1;
            int right = (cur.right != null && (cur.right.val - 1) == cur.val) ? map.get(cur) + 1 : 1;
            
            max = Math.max(max, Math.max(left, right));
            if (cur.right != null) {
                stack.push(cur.right);
                map.put(cur.right, right);
            }
            
            if (cur.left != null) {
                stack.push(cur.left);
                map.put(cur.left, left);
            }
        }
        
        return max;
    }
}
```

使用遞迴去作，此連續子序列是嚴格遞增的，所以必須記住parent的值，與當前的值作比較，如果當前值比parent少一，則len++，否則len設為1重新往下找，因左子樹或是右子樹有可能有更比len更長的子序列，因此最後需要與左右子樹遞迴得到的值與當前的len作比較，取最長的，程式碼如下：

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
    public int longestConsecutive(TreeNode root) {
        return helper(root, null, 0);
    }
    
    private int helper(TreeNode node, TreeNode parent, int len) {
        if (node == null) {
            return len;
        }
        
        len = (parent != null && parent.val + 1 == node.val) ? len + 1 : 1;
        int left = helper(node.left, node, len);
        int right = helper(node.right, node, len);
        int max = Math.max(len, Math.max(left, right));
        
        return max;
    }
}
```

---
###Reference
1. http://www.cnblogs.com/jcliBlogger/p/4923745.html
2. https://leetcode.com/discuss/66548/recursive-solution-bottom-iteration-solution-using-stack