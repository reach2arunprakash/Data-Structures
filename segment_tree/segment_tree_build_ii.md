#Segment Tree Build II

[原題連結](http://www.lintcode.com/en/problem/segmemt-tree-build-ii/)

題意：為[Segment Tree Build I](segment_tree/segment_tree_build_i.md) 的延伸，此為max segment tree，多一個max欄位來儲存該區段的最大值。

解題思路：程式碼如下


```java
public class Solution {
    /**
     *@param A: a list of integer
     *@return: The root of Segment Tree
     */
    public SegmentTreeNode build(int[] A) {
        
        if (A == null || A.length == 0) {
            return null;
        }
        
        return buildST(A, 0, A.length - 1);
    }
    
    private SegmentTreeNode buildST(int[] A, int start, int end) {
        if (start > end) {
            return null;
        }
        
        SegmentTreeNode root;
        if (start == end) {
            root = new SegmentTreeNode(start, end, A[start]);
        } else {
            int mid = start + (end - start) /2;
            root = new SegmentTreeNode(start, end, 0);
            root.left = buildST(A, start, mid);
            root.right = buildST(A, mid + 1, end);
            root.max = Math.max(root.left.max, root.right.max);
        }
        
        return root;
    }
}
```