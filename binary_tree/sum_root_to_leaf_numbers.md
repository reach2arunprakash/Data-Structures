# Sum Root to Leaf Numbers

[Leetcode](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

題意：

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,
```
    1
   / \
  2   3
```
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25.


解題思路：

使用遞迴去作，因為java無法 pass by reference，因此傳一個陣列下去，其程式碼如下：

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
    public int sumNumbers(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int[] sum = {0};
        int current = 0;
        helper(root, current, sum);
        
        return sum[0];
    }
    
    private void helper(TreeNode root, int current, int[] sum) {
        if (root == null) {
            return;
        }
        
        current = current * 10 + root.val;
        if (root.left == null && root.right == null) {
            sum[0] = sum[0] + current;
            return;
        }
        
        helper(root.left, current, sum);
        helper(root.right, current, sum);
    }
}
```
---
###Reference
1. http://rleetcode.blogspot.com/2014/03/sum-root-to-leaf-numbersjava-and-python.html