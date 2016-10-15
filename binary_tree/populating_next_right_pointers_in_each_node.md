# Populating Next Right Pointers in Each Node

[Leetcode](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

題意：


Given a binary tree
```
struct TreeLinkNode {
    TreeLinkNode *left;
    TreeLinkNode *right;
    TreeLinkNode *next;
}
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

You may only use constant extra space.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
For example,
Given the following perfect binary tree,
```
     1
   /  \
  2    3
 / \  / \
4  5  6  7
    ```
After calling your function, the tree should look like:
```
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \  / \
4->5->6->7 -> NULL
```
解題思路：

用dfs加遞迴去作

>左節點的next指向root的右節點或是null
>右子樹的節點指向null或是root的next的左節點

程式碼如下：

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        if (root == null) {
            return;
        }
        root.next = null;
        helper(root);
    }
    
    public void helper(TreeLinkNode root) {
        if (root == null) {
            return;
        }
        
        if (root.left != null) {
            root.left.next = (root.right == null) ? null : root.right;
        }
        
        if (root.right != null) {
            root.right.next = (root.next == null) ? null : root.next.left;
        }
        
        helper(root.left);
        helper(root.right);
    }
}
```

非遞迴解法

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        if (root == null) {
            return;
        }
        
        // p 記錄當前的節點
        // first紀錄下一層的最左邊那個節點，下次走訪從那邊開始走。
        TreeLinkNode p = root;
        TreeLinkNode first = null;
        while (p != null) {
            // 如果first是空的，則指向左邊的節點
            if (first == null) {
                first = p.left;
            }
            
            if (p.left != null) {
                p.left.next = p.right;
            } else {
                break; // 若為空了，表示下面一定也沒有值可以走了，直接break
            }
            
            // 如果next不為空，因為為complete bt，所以表示當下的右節點一定有值，
            // 把p.next.left 指向p.right.next
            if (p.next != null) {
                p.right.next = p.next.left;
                p = p.next;
                continue;
            } else {
                p = first;
                first = null;
            }
        }
    }
}
```

