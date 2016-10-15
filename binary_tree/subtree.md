#Subtree

[]()

題意：給定兩棵樹 T1 與 T2 ，檢查 T2 是否為 T1 的子樹。


解題思路：使用 Divide and Conquer 來解這道題，其中還需要另一個副程式來檢查 以 T1 的根節來遞迴檢查是否相等，首先先檢查以 T1 為根的樹是否含有與 T2 相同的子樹，若不是的話，則再往 T1 的左節點再往下遞迴，再找不到的話，就再往 T1 的右節點找，其程式碼如下：

```java
public class Solution {
    /**
     * @param T1, T2: The roots of binary tree.
     * @return: True if T2 is a subtree of T1, or false.
     */
    public boolean isSubtree(TreeNode T1, TreeNode T2) {
        if (T2 == null) {
            return true;
        }
        
        if (T1 == null) {
            return false;
        }
        
        return isSameTree(T1, T2) || isSubtree(T1.left, T2) || isSubtree(T1.right, T2);
    }
    
    private boolean isSameTree(TreeNode T1, TreeNode T2) {
        if (T1 == null && T2 == null) {
            return true;
        }
        
        if (T1 == null || T2 == null || T1.val != T2.val) {
            return false;
        }
        
        return isSameTree(T1.left, T2.left) && isSameTree(T1.right, T2.right);
    }
}
```

---
###Reference
1. 劍指 Offer 第三章
2. http://cherylintcode.blogspot.com/2015/06/subtree.html
3. 