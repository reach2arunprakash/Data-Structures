# Binary Tree Right Side View

[Leetcode](https://leetcode.com/problems/binary-tree-right-side-view/)

題意：

Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

For example:
Given the following binary tree,
```
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
You should return [1, 3, 4].
```

解題思路：

用層遍歷來走訪整個二元樹，右節點先加，每次第一個pop出來的節點便加入res中。



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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root == null) {
            return res;
        }
        
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(root);
        while(!q.isEmpty()) {
            int size = q.size();
            for(int i = 0; i < size; i++) {
                TreeNode top = q.poll();
                if (i == 0) {
                    res.add(top.val);
                }
                if (top.right != null) {
                    q.offer(top.right);
                }
                if (top.left != null) {
                    q.offer(top.left);
                }
            }
        }
        
        return res;
    }
}
```

---
###Reference
1. http://bookshadow.com/weblog/2015/04/03/leetcode-binary-tree-right-side-view/