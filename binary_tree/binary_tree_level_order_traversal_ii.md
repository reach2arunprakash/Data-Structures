# Binary Tree Level Order Traversal II

[Leetcode](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)

題意：

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

**For example:**

Given binary tree ```{3,9,20,#,#,15,7}```,
```
    3
   / \
  9  20
    /  \
   15   7
   ```
return its bottom-up level order traversal as:
```
[
  [15,7],
  [9,20],
  [3]
]
```


解題思路：

使用linked list的特性，插入節點(add first)只需要O(1)的時間。


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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        LinkedList<List<Integer>> result = new LinkedList<List<Integer>>();
        if(root == null){
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int queueLen = queue.size();
            List<Integer> curRowResult = new ArrayList<Integer>();
            for(int i = 0 ; i< queueLen; i++){
                TreeNode curElem = queue.poll();
                curRowResult.add(curElem.val);
                if(curElem.left != null){
                    queue.offer(curElem.left);
                }
                if(curElem.right != null){
                    queue.offer(curElem.right);
                }
            }
            result.addFirst(curRowResult);
        }
    
        return result;
    }
}
```