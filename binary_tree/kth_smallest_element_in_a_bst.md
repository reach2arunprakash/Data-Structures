#Kth Smallest Element in a BST

[原題網址](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

題意：找出 BST 中第 k 大的元素。


解題思路：

暴力法花 O(N)，利用中序遍歷樹中每個節點後存到一個 list， 接著把list中第k個元素抓出來即可。

```java
public class Solution {
    public int kthSmallest(TreeNode root, int k) {
        
        if (root == null) {
            return 0;
        }
        
        List<Integer> res = new ArrayList<Integer>();
        helper(res, root);
        
        if (k > res.size()) {
            return 0;
        } else {
            return res.get(k - 1);
        }
        
    }
    
    private void helper(List<Integer> list, TreeNode root) {
        if (root == null) {
            return;
        }
        
        helper(list, root.left);
        list.add(root.val);
        helper(list, root.right);
    }
}
```

---
O(logn + K) 法，一旦找到第k個值即返回，不繼續往下找，節省許多時間，程式碼如下：

```java
public class Solution {
    
    int kth;
    int res;
    public int kthSmallest(TreeNode root, int k) {
        
        if (root == null || k == 0) {
            return 0;
        }
        
        kth = k;
        
        helper(root);
        return res;
    }
    
    private void helper(TreeNode root) {
        if (root == null) {
            return;
        }
        
        helper(root.left);
        
        kth--;
        if (kth == 0) {
            res = root.val;
            return;
        }
        
        helper(root.right);
    }
}
```

---
###Reference
1. https://leetcode.com/discuss/51806/python-recursive-solution-averaged-time-o-lg-n-k