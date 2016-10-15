#Binary Tree Zigzag Level Order Traversal

[原題網址](http://www.lintcode.com/en/problem/binary-tree-zigzag-level-order-traversal/)

題意：

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).


解題思路：使用level order traversal，再加上queue與stack的幫助。

程式碼如下，但有錯誤，待發現bug再回頭修正

```java
public ArrayList<ArrayList<Integer>> zigzagLevelOrder(TreeNode root) {
    
    ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
    if (root == null) {
        return null;
    }
    
    int level = 1;
    Stack<TreeNode> stack = new Stack<TreeNode>();
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    Queue<TreeNode> traverseQ = new LinkedList<TreeNode>();
    
    traverseQ.offer(root);
    while (!traverseQ.isEmpty()) {
        int size = traverseQ.size();
        
        for (int i = 0; i < size; i++) {
            TreeNode cur = traverseQ.poll();
            if (level % 2 == 1) {
                queue.offer(cur);
            } else {
                stack.push(cur);
            }
            
            if (cur.left != null) {
                traverseQ.offer(cur.left);
            }
            
            if (cur.right != null) {
                traverseQ.offer(cur.right);
            }
        }
        
        List<Integer> tempRes = new ArrayList<Integer>();
        if (level % 2 == 1) {
            for (int i = 0; i < queue.size(); i++) {
                tempRes.add(queue.poll().val);
            }
        } else {
            for (int i = 0; i < stack.size(); i++) {
                tempRes.add(stack.pop().val);
            }
        }
        
        res.add(new ArrayList<Integer>(tempRes));
        level++;
    }
    
    return res;
}
```

---
網友 [喜刷刷](http://bangbingsyb.blogspot.com/2014/11/leetcode-binary-tree-zigzag-level-order.html) 的解法

解題思路：利用兩個 Stack 不斷的交替使用，由於 Stack 是先進後出，籍由判斷 ```leftToRight``` 變數來決定存放的順序，接著再把兩個 stack 交換。

```java
public ArrayList<ArrayList<Integer>> zigzagLevelOrder(TreeNode root) {
    
    ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
    
    if (root == null) {
        return res;
    }
    
    Stack<TreeNode> curLevel = new Stack<TreeNode>();
    Stack<TreeNode> nextLevel = new Stack<TreeNode>();
    curLevel.push(root);
    boolean leftToRight = true;
    
    while (!curLevel.isEmpty()) {
        ArrayList<Integer> tempRes = new ArrayList<Integer>();
        
        while (!curLevel.isEmpty()) {
            TreeNode cur = curLevel.pop();
            tempRes.add(cur.val);
            
            if (leftToRight) {
                if (cur.left != null) {
                    nextLevel.push(cur.left);
                }
                if (cur.right != null) {
                    nextLevel.push(cur.right);
                }
            } else {
                if (cur.right != null) {
                    nextLevel.push(cur.right);
                }
                if (cur.left != null) {
                    nextLevel.push(cur.left);
                }
            }
        }
        res.add(tempRes);
        Stack<TreeNode> temp = curLevel;
        curLevel = nextLevel;
        nextLevel = temp;
        leftToRight = !leftToRight;
        
    }
    
    return res;
}
```
---

另一解法：仍然按照level order traversal來遍歷整顆樹，接下來再把對應的列來作反轉的動作，時間複雜度較高。

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
    List<List<Integer>> res;
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        res = new ArrayList<List<Integer>>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> list = new ArrayList<Integer>();
            for (int i = 0; i < size; i++) {
                TreeNode current = queue.poll();
                list.add(current.val);
                if (current.left != null) {
                    queue.offer(current.left);
                }
                if (current.right != null) {
                    queue.offer(current.right);
                }
            }
            res.add(new ArrayList<Integer>(list));
        }
        
        for (int i = 1; i < res.size(); i = i + 2) {
            List<Integer> list = reverse(res.get(i));
            res.set(i, list);
        }
        return res;
    }
    
    private List<Integer> reverse(List<Integer> list) {
        int start = 0;
        int end = list.size() - 1;
        while (start < end) {
            int temp = list.get(start);
            list.set(start, list.get(end));
            list.set(end, temp);
            start++;
            end--;
        }
        return list;
    }
}
```