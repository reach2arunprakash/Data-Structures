# Recover Binary Search Tree

[]()

題意：

Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.


解題思路：

對於bst作中序遍歷，使用兩個list，分別紀錄節點與節點的值，最後再根據對應的位置 assign 對應的值，時間複雜度為O(NlogN)，空間複雜度為O(N)，其程式碼如下：


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
    List<TreeNode> nodes = new ArrayList<TreeNode>();
    List<Integer> values = new ArrayList<Integer>();
    public void recoverTree(TreeNode root) {
        if (root == null) {
            return;
        }
        inorderTraversal(root);
        Collections.sort(values);
        for (int i = 0; i < nodes.size(); i++) {
            nodes.get(i).val = values.get(i);
        }
    }
    
    
    public void inorderTraversal(TreeNode root) {
        if (root == null) {
            return;
        }
        inorderTraversal(root.left);
        nodes.add(root);
        values.add(root.val);
        inorderTraversal(root.right);
        
    }
}
```

另有O(1)空間複雜度與O(N)時間複雜度的解法

---
###Reference
1. http://fisherlei.blogspot.com/2012/12/leetcode-recover-binary-search-tree.html