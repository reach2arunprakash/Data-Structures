#Segment Tree Build I
只建區間，尚未回溯回填該區間的最大或最小值

[原題網址](http://www.lintcode.com/en/problem/segment-tree-build/)

```java
public SegmentTreeNode build(int start, int end) {
    
    //記得處理corner case，否則會無限遞迴
    if (start > end) {
        return null;
    }
    
    // start == end代表達到leaf，直接產生一個新leaf回傳即可
    if (start == end) {
        SegmentTreeNode newNode = new SegmentTreeNode(start, end);
        return newNode;
    }
    
    // 否則還要繼續遞迴下去，分別遞迴左右子樹之後再回傳
    int mid = (start + end) / 2;
    SegmentTreeNode root = new SegmentTreeNode(start, end);
    root.left = build(start, mid);
    root.right = build(mid + 1, end);
    
    return root;

}

```
---
#Segment Tree Build II
除了與第一部份一樣之外，還需回傳該區間的最大或最小值

