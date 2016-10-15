# Validate Binary Search Tree

[原題網址](http://www.lintcode.com/en/problem/validate-binary-search-tree/)

> Given a binary tree, determine if it is a valid binary search tree (BST).


updated on 2015.12.25

因為leetcode有些corner case，我們使用integer物件來作：

```java
public class Solution {
    public boolean isValidBST(TreeNode root) {
        return helper(root, null, null);
    }

    boolean helper(TreeNode root, Integer min, Integer max) {
        if (root == null)
            return true;

        if ((min != null && root.val <= min) || (max != null && root.val >= max))
            return false;

        return helper(root.left, min, root.val) && helper(root.right, root.val, max);
    }
}
```


不是單單查找左右子節點是否符合即可，得需要檢查特定的範圍
因此用了lower與upper來確保值是正確的。
```java
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: True if the binary tree is BST, or false
     */
    public boolean isValidBST(TreeNode root) {
        // write your code here
        if (root == null) {
            return true;
        }

        return isBST(root.left, Integer.MIN_VALUE, root.val) &&
                isBST(root.right, root.val, Integer.MAX_VALUE);
    }

    public boolean isBST(TreeNode root, int lower, int upper) {
        if (root == null) {
            return true;
        }
        if (root.val > lower && root.val < upper) {
            return isBST(root.left, lower, root.val) &&
                    isBST(root.right, root.val, upper);
        } else {
            return false;
        }
    }
}
```
```java
     
    public class ResultType {
        boolean isValidBST;
        int max, min;
        
        ResultType(boolean isValidBST, int max, int min) {
            this.isValidBST = isValidBST;
            this.max = max;
            this.min = min;
        }
    }
    
    public boolean isValidBST(TreeNode root) {
        // write your code here
        if ( root == null ) {
            return true;
        }
        
        ResultType result = helper(root, Integer.MAX_VALUE, Integer.MIN_VALUE);
        
        return result.isValidBST;
    }
    
    public ResultType helper(TreeNode root, int max, int min) {
        if ( root == null ) {
            return new ResultType(true, max, min );
        }
        
        ResultType result = new ResultType(true, Integer.MAX_VALUE, Integer.MIN_VALUE );
        
        ResultType left = helper(root.left, root.val-1, min);
        ResultType right = helper(root.right, max, root.val+1);
        
        if ( !left.isValidBST || !right.isValidBST || root.val > max || root.val < min) {
            result.isValidBST = false;
            return result;
        }
        
        return result; 
    }
    
```