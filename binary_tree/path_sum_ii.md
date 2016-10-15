# Path Sum II

[Leetcode](https://leetcode.com/problems/path-sum-ii/)

題意：

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
return
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

解題思路：

使用dfs去作，要特別注意刪加元素的時機，一旦加了元素，一定要刪掉，否則會有重複元素在裡面，程式碼如下：

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
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        if (root == null) {
            return res;
        }
        helper(root, sum, new ArrayList<Integer>());
        return res;
    }
    
    private void helper(TreeNode root, int sum, List<Integer> temp) {
        if (root == null) {
            return;
        }
        
        if (root.left == null && root.right == null) {
            if (sum == root.val) {
                temp.add(root.val);
                res.add(new ArrayList<Integer>(temp));
                temp.remove(temp.size() - 1);
            }
            return;
        }
        temp.add(root.val);
        helper(root.left, sum - root.val, temp);
        helper(root.right, sum - root.val, temp);
        temp.remove(temp.size() - 1);
    }
}
```