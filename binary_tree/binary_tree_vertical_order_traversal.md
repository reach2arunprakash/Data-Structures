# Binary Tree Vertical Order Traversal

[]()

題意：

Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from left to right.

Examples:
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its vertical order traversal as:
```
[
  [9],
  [3,15],
  [20],
  [7]
]
```
Given binary tree [3,9,20,4,5,2,7],
```
    _3_
   /   \
  9    20
 / \   / \
4   5 2   7
```
return its vertical order traversal as:
```
[
  [4],
  [9],
  [3,5,2],
  [20],
  [7]
]
```

解題思路：

我們利用兩個hashmap來分別記錄該column的所有樹節點，與各個樹節點應該在哪個column，接著利用bfs的不斷走訪每個節點。

假設當前的root的column 為 w，則左節點的column為 w - 1，且右節點的column 為w + 1。

最後再各個column的list全加進res即可，其程式碼如下：

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
    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        
        Map<Integer, List<Integer>> map = new HashMap<>();
        Map<TreeNode, Integer> weight = new HashMap<>();
        Queue<TreeNode> q = new LinkedList<>();
        
        q.offer(root);
        weight.put(root, 0);
        
        int min = 0;
        int max = 0;
        while (!q.isEmpty()) {
            TreeNode cur = q.poll();
            
            int w = weight.get(cur);
            if (!map.containsKey(w)) {
                map.put(w, new ArrayList<Integer>());
            }
            map.get(w).add(cur.val);
            
            if (cur.left != null) {
                q.offer(cur.left);
                weight.put(cur.left, w - 1);
                min = Math.min(w - 1, min);
            }
            
            if (cur.right != null) {
                q.offer(cur.right);
                weight.put(cur.right, w + 1);
                max = Math.max(w + 1, max);
            }
        }
        
        for (int i = min; i <= max; i++) {
            res.add(map.get(i));
        }
        
        return res;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/73113/using-hashmap-bfs-java-solution