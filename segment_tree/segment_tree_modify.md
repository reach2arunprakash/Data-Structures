#Segment Tree Modify

找到需要更改的葉節點，並將更新後的最大值回遡上來

[原題網址](http://www.lintcode.com/en/problem/segment-tree-modify/)
```java
public void modify(SegmentTreeNode root, int index, int value) {
    
    if (root.start == index && root.end == index) {
        root.max = value;
        return;
    }
    
    int mid = (root.start + root.end) / 2;
    
    if (index <= mid) {
        modify(root.left, index, value);
    } else {
        modify(root.right, index, value);
    }
    
    root.max = Math.max(root.left.max, root.right.max);
}
```