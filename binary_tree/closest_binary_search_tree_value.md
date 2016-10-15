# Closest Binary Search Tree Value

[Leetcode](https://leetcode.com/problems/closest-binary-search-tree-value/)

題意：

給一個target，找一個最接近target的節點


Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

Note:

Given target value is a floating point.
You are guaranteed to have only one unique value in the BST that is closest to the target.



解題思路：

updated 2015.12.5

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
    double minDistance = Double.MAX_VALUE;
    int res = 0;
    public int closestValue(TreeNode root, double target) {
        if (root == null) {
            return 0;
        }
        helper(root, target);
        return res;
    }
    
    private void helper(TreeNode root, double target) {
        if (root == null) {
            return;
        }
        
        if (Math.abs(root.val - target) < minDistance) {
            minDistance = Math.abs(root.val - target);
            res = root.val;
        }
        
        if (target > root.val) {
            helper(root.right, target);
        } else {
            helper(root.left, target);
        }
        
        
    }
}
```

法一：遞迴

不斷的拿diff去比較，回傳差距最小的那個節點的值

程式碼如下：

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
    public int closestValue(TreeNode root, double target) {
        if (root == null) {
            return Integer.MAX_VALUE;
        } 
        if (root.left == null && root.right == null) {
            return root.val;
        }
        
        int left = closestValue(root.left, target);
        int right = closestValue(root.right, target);
        double rootDiff = Math.abs(root.val - target);
        double leftDiff = Math.abs(target - left);
        double rightDiff = Math.abs(target - right);
        
        if (rootDiff < leftDiff) {
            return (rootDiff < rightDiff) ? root.val : right;
        } else {
            return (leftDiff < rightDiff) ? left : right;
        }
    }
}
```

法二：非遞迴作法

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
    public int closestValue(TreeNode root, double target) {
        if (root == null) {
            return 0;
        }
        int closestVal = root.val;
        while(root != null) {
            // 比較原本的比較接近target還是root
            closestVal = (Math.abs(target - root.val) < Math.abs(target - closestVal)) ? root.val : closestVal;
            // 表示找到，直接返回即可
            if (closestVal == target) {
                return closestVal;
            }
            // 再根據target與root的關係來決定往左往右走
            root = (root.val > target) ? root.left : root.right;
        }
        
        return closestVal;
    }
}
```

---
###Reference
1. https://leetcode.com/discuss/55460/simple-iterative-java-solution-with-explaination