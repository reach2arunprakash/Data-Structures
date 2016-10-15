#Segment Tree Query II

此為計數型線段樹，

```java
public int query(SegmentTreeNode root, int start, int end) {
    
    if (start > end || root == null) {
        return 0;
    }
    
    //注意這裡，與傳統型線段樹不同！
    if (start <= root.start && root.end <= end) {
        return root.count;
    }
    
    int mid = (root.start + root.end) / 2;
    int leftCount = 0;
    int rightCount = 0;
    
    if (start <= mid) {
        if (mid < end) {
            leftCount = query(root.left, start, mid);
        } else {
            leftCount = query(root.left, start, end);
        }
    }
    
    if (mid < end) {
        if (start <= mid) {
            rightCount = query(root.right, mid + 1, end);
        } else {
            rightCount = query(root.right, start, end);
        }
    }
    
    return leftCount + rightCount;
    
}
```