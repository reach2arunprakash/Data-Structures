#Convert Sorted Array to Binary Search Tree With Minimal Height

[原題連結](http://www.lintcode.com/en/problem/convert-sorted-array-to-binary-search-tree-with-minimal-height/#)

題意：利用排序好的陣列建出平衡二元搜尋樹

解題思路：用遞迴作即可，但需注意分割時的邊界，左子樹為start 到 mid -1 ，右子樹為 mid + 1 到 end ，程式碼如下：

```java
public class Solution {
    /**
     * @param A: an integer array
     * @return: a tree node
     */
    public TreeNode sortedArrayToBST(int[] A) {  
        if (A == null || A.length == 0) {
            return null;
        }
        
        return build(A, 0, A.length - 1);
    }  
    
    private TreeNode build(int[] A, int start, int end) {
        if (start > end) {
            return null;
        }
        
        int mid = start + (end - start) / 2;
        TreeNode root = new TreeNode(A[mid]);
        
        root.left = build(A, start, mid-1);
        root.right = build(A, mid+1, end);
        
        return root;
    }
}
```