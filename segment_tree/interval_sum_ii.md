#Interval Sum II

[原題連結](http://www.lintcode.com/en/problem/interval-sum-ii/)

與第一題相同，唯一不同的地方是陣列的值會改變，若使用原始的暴力法或排序搜尋法複雜度會相當高，在此用線段樹來實作，可達到查詢與修改只要 O(logN) 的複雜度。

update on 2015.12.8

```java
class SegTreeNode{
    int start, end;
    int sum;
    SegTreeNode lson, rson;
    public SegTreeNode(int start, int end){
        this.start = start;
        this.end = end;
        lson = null;
        rson = null;
        this.sum = 0;
    }
}

class SegTree{
    SegTreeNode root = null;
    public SegTree(int[] nums){
        root = buildTree(nums, 0, nums.length - 1);
    }

    private SegTreeNode buildTree(int[] nums, int start, int end){
        if(start > end) return null;
        SegTreeNode ret = new SegTreeNode(start, end);
        if(start == end){
            ret.sum = nums[start];
            return ret;
        }    
        int mid = start + (end - start)/2;
        ret.lson = buildTree(nums, start, mid);
        ret.rson = buildTree(nums, mid + 1, end);
        ret.sum = ret.lson.sum + ret.rson.sum;
        return ret;
    }

    public void update(int pos, int val) {
        update(root, pos, val);
    }

    private void update(SegTreeNode root, int pos, int val){
        if(root.start == root.end){
            root.sum = val;
            return;
        }
        int mid = root.start + (root.end - root.start)/2;
        if(pos <= mid)
            update(root.lson, pos, val);
        else
            update(root.rson, pos, val);
        root.sum = root.lson.sum + root.rson.sum;
    }

    public int sumRange(int i, int j){
        SegTreeNode cur = root;
        return sumRange(cur, i, j);
    }

    private int sumRange(SegTreeNode root, int start, int end){
        if(root.start == start && root.end == end)
            return root.sum;
        int mid = root.start + (root.end - root.start)/2;
        if(start >= mid + 1)
            return sumRange(root.rson, start, end);
        else if(end <= mid)
            return sumRange(root.lson, start, end);
        return sumRange(root.lson, start, mid) + sumRange(root.rson, mid + 1, end);
    }
}

public class NumArray {
    SegTree tree;
    public NumArray(int[] nums) {
        tree = new SegTree(nums);
    }

    public void update(int i, int val) {
        tree.update(i, val);
    }

    public int sumRange(int i, int j) {
        return tree.sumRange(i, j);
    }
}
```

2015.9.15

```java
public class Solution {
    
    class SegmentTreeNode {
        public int start, end, sum;
        public SegmentTreeNode left, right;
        public SegmentTreeNode(int start, int end, int sum) {
            this.start = start;
            this.end = end;
            this.sum = sum;
            this.left = null;
            this.right = null;
        }
    }
    

    /**
     * @param A: An integer array
     */
    SegmentTreeNode root;
    public Solution(int[] A) {
        root = buildST(0, A.length - 1, A);
    }
    
    
    public SegmentTreeNode buildST(int start, int end, int[] A) {
        
        if (start > end) {
            return null;
        }
        
        SegmentTreeNode root = new SegmentTreeNode(start, end, 0);
        
        if (start == end) {
            root.sum = A[start];
        } else {
            int mid = (start + end) / 2;
            root.left = buildST(start, mid, A);
            root.right = buildST(mid + 1, end, A);
            root.sum = root.left.sum + root.right.sum;
        }
        
        return root;
    }
    
    /**
     * @param start, end: Indices
     * @return: The sum from start to end
     */
    public long query(int start, int end) {
        
        return queryST(root, start, end);
    }
    
    public int queryST(SegmentTreeNode root, int start, int end) {
        
        if (root.start == start && root.end == end) {
            return root.sum;
        }
        
        int mid = (root.start + root.end) / 2;
        int leftSum = 0;
        int rightSum = 0;
        
        if (start <= mid) {
            if (end > mid) {
                leftSum = queryST(root.left, start, mid);
            } else {
                //left right要搞清楚！！
                leftSum = queryST(root.left,start, end);
            }
        }
        
        if (end > mid) {
            if (start <= mid) {
                rightSum = queryST(root.right, mid + 1, end);
            } else {
                rightSum = queryST(root.right, start, end);
            }
        }
        
        return leftSum + rightSum;
    }
    
    /**
     * @param index, value: modify A[index] to value.
     */
    public void modify(int index, int value) {
        modifyST(root, index, value);
    }
    
    public void modifyST(SegmentTreeNode root, int index, int value) {
        
        if (root.start == index && root.end == index) {
            root.sum = value;
            return;
        }
        
        int mid = (root.start + root.end) / 2;
        
        if (index <= mid) {
            modifyST(root.left, index, value);
        } else {
            modifyST(root.right, index, value);
        }
        
        root.sum = root.left.sum + root.right.sum;
    }
}


```