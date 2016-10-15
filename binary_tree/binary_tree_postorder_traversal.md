# Binary Tree Postorder Traversal

[Leetcode](https://leetcode.com/problems/binary-tree-postorder-traversal/)

題意：

Given a binary tree, return the postorder traversal of its nodes' values.

For example:
Given binary tree {1,#,2,3},
```
   1
    \
     2
    /
   3
   ```
return [3,2,1].


解題思路：


updated on 2015.12.30

使用linked list的特性，不斷把值加到list的最前端即可，其餘作法都與preorder一樣。插入的時間仍是o(1)

```java
public List<Integer> postorderTraversal(TreeNode root) {
    LinkedList<Integer> ans = new LinkedList<>();
    Stack<TreeNode> stack = new Stack<>();
    if (root == null) return ans;

    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode cur = stack.pop();
        ans.addFirst(cur.val);
        if (cur.left != null) {
            stack.push(cur.left);
        }
        if (cur.right != null) {
            stack.push(cur.right);
        } 
    }
    return ans;
}
```

網友[胖虎](http://blog.csdn.net/ljphhj/article/details/21369053) 提供以下思路，此法會破壞樹的結構：

"我們寫一個while循環，while循環結束的條件是棧中沒有任何結點。

當棧中有結點的時候，我們將取出棧頂結點

1、判斷下它是否是葉子結點（left和right都為null）, 如果是葉子結點的話，那麼不好意思，把它彈出棧，並把值add到ArrayList中，如果不是葉子結點的話，那麼

2、我們判斷下它的left（node.left）是否為null,如果不為null，把它的左孩子結點push到棧中來，並把它的左孩子域設為null, 然後跳過此次循環剩下的部分

3、如果它的left 為null, 把它的右孩子結點push到棧中來，並把它的右孩子域設為null，然後跳過此次循環剩下的部份
"

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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root == null) {
            return res;
        }
        
        Stack<TreeNode> s = new Stack<TreeNode>();
        s.push(root);
        while (!s.isEmpty()) {
            TreeNode cur = s.peek();
            if (cur.left == null && cur.right == null) {
                res.add(cur.val);
                s.pop();
            }
            if (cur.left != null) {
                s.push(cur.left);
                cur.left = null;
            } else if (cur.right != null) {
                s.push(cur.right);
                cur.right = null;
            }
        }
        
        return res;
    }
}
```
---
###Reference
1. http://blog.csdn.net/ljphhj/article/details/21369053