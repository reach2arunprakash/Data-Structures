# Count Complete Tree Nodes

[Leetcode](https://leetcode.com/problems/count-complete-tree-nodes/)

題意：

Given a complete binary tree, count the number of nodes.


解題思路：


updated 2015.12.1

遞迴的作法，分別算左右子樹的高度，若高度相同，則此樹是full tree，則直接返回2^h-1即可，否則繼續遞迴下報

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
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int left = leftDepth(root);
        int right = rightDepth(root);
        if (left == right) {
            return (1 << left) - 1;
        } else {
            return countNodes(root.left) + countNodes(root.right) + 1;
        }
    }
    
    private int leftDepth(TreeNode root) {
        int depth = 0;
        while (root != null) {
            depth++;
            root = root.left;
        }
        return depth;
    }
    
    private int rightDepth(TreeNode root) {
        int depth = 0;
        while (root != null) {
            depth++;
            root = root.right;
        }
        return depth;
    }
}
```

網友 [書影](http://bookshadow.com/weblog/2015/06/06/leetcode-count-complete-tree-nodes/) 提供了以下思路：

遞迴法：分別列舉左右子樹的高度，如果兩者一樣，表示此子樹為full bt，因此直接返回 $$2^h - 1$$即可，否則繼續往下遞迴，此法複雜度為指數型，leetcode無法通過。

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
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        TreeNode leftNode = root;
        TreeNode rightNode = root;
        int leftHeight = 0;
        int rightHeight = 0;
        while(leftNode != null) {
            leftNode = leftNode.left;
            leftHeight++;
        }
        while(rightNode != null) {
            rightNode = rightNode.right;
            rightHeight++;
        }
        
        if (rightHeight == leftHeight) {
            return (int)Math.pow(2, rightHeight) - 1; // 因pow會回傳double
        } else {
            return countNodes(root.left) + countNodes(root.right) + 1; //加上root本身的那個節點，因此加一
        }
    }
}
```
---
參考了網友 [Jikai Tang](http://www.tangjikai.com/algorithms/leetcode-222-complete-tree-nodes) 的作法，不是很好理解最後二分的部份，待理解後再來補全思路。

二分搜尋法：首先找出樹的總高度，接著來算出最後一層如果是full bt的話該有多少節點，接著再不斷的去作二分來找出最後一層的最後一個節點在哪裡。

>num - 1 表示為由第1層到第h-1層的節點總數，最後再找出二分法後找出的l在哪裡
>總節點數即為 num - 1 + L

其程式碼如下：

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
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        TreeNode temp = root;
        int depth = 0;
        while(temp != null) {
            depth++;
            temp = temp.left;
        }
        
        // 找出最底層如果是full bt的話該有多少個子節點
        int num = (int)Math.pow(2, depth - 1);
        int l = 0;
        int r = num - 1;
        
        // 表示為full的，直接把1到h-1層的節點數全加上並返回即可
        if (getDepth(root, r, num) == depth) {
            return num * 2 - 1;
        }
        
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (getDepth(root, mid, num) == depth) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        
        return (num - 1) + l;
    }
    
    // 此method是要找出值為idx的節點在樹中的第幾層
    private int getDepth(TreeNode root, int idx, int num) {
        int depth = 0;
        
        while (root != null) {
            depth += 1; //往下找因此depth加一
            num /= 2; // 找出中間index值
            
            // 作binary search
            if (idx >= num) {
                root = root.right;
                idx -= num; // 因idx比num大，因此往右邊找idx為 idx -= num的，
            } else {
                root = root.left;
            }
        }
        return depth;
    }
}
```


---
###Reference
1. http://bookshadow.com/weblog/2015/06/06/leetcode-count-complete-tree-nodes/
2. http://www.tangjikai.com/algorithms/leetcode-222-complete-tree-nodes