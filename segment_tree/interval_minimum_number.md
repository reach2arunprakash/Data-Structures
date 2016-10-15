#Interval Minimum Number

[原題連結](http://www.lintcode.com/en/problem/interval-minimum-number/)

題意：給一個陣列，與一連串的區間，找出各個區間的最小值。

解題思路：此題透過建立線段樹，接著再不斷查詢各區間的最小值即可，大部份的程式碼都是實作線段樹，其正查詢的code。

```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */
public class Solution {
    /**
     *@param A, queries: Given an integer array and an query list
     *@return: The result list
     */
    class SegmentTreeNode {
        int start; 
        int end;
        int min;
        SegmentTreeNode left;
        SegmentTreeNode right;
        
        public SegmentTreeNode(int start, int end) {
            this.start = start;
            this.end = end;
            this.min = Integer.MAX_VALUE;
            this.left = null;
            this.right = null;
        }
    }
    
    private void modifyST(SegmentTreeNode root, int index, int value) {
        
        if (root.start == index && root.end == index) {
            root.min = value;
            return;
        }
        
        int mid = (root.start + root.end) / 2;
        
        if (index <= mid) {
            modifyST(root.left, index, value);
        } else {
            modifyST(root.right, index, value);
        }
        
        root.min = Math.min(root.left.min, root.right.min);
    }
    
    private SegmentTreeNode buildST (int A[], int start, int end) {
        
        if (start > end) {
            return null;
        }
        
        SegmentTreeNode root = new SegmentTreeNode(start, end);
        
        if (start == end) {
            root.min = A[start];
            return root;
        }
        
        int mid = (start + end) / 2;
        
        root.left = buildST(A, start, mid);
        root.right = buildST(A, mid + 1, end);
        root.min = Math.min(root.left.min, root.right.min);
        
        return root;
    }
    
    private int queryST (SegmentTreeNode root, int start, int end) {
        
        if (start == root.start && end == root.end ) {
            return root.min;
        }
        
        int mid = (root.start + root.end) / 2;
        int leftMin = Integer.MAX_VALUE;
        int rightMin = Integer.MAX_VALUE;
        
        if (start <= mid) {
            if (end > mid) {
                leftMin = queryST(root.left, start, mid);
            } else {
                leftMin = queryST(root.left, start, end);
            }
        }
        
        if (end > mid) {
            if (start <= mid) {
                rightMin = queryST(root.right, mid + 1, end);
            } else {
                rightMin = queryST(root.right, start, end);
            }
        }
        
        return Math.min(leftMin, rightMin);
        
    }
    
    public ArrayList<Integer> intervalMinNumber(int[] A, 
                                                ArrayList<Interval> queries) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        
        if (A == null || A.length == 0) {
            return res;
        }
        
        SegmentTreeNode root = buildST(A, 0, A.length - 1);
        for (Interval query : queries) {
            int min = queryST(root, query.start, query.end);
            res.add(min);
        }
        
        return res;
        
    }
}

```

>若一開始使用暴力法的話，會需要花O(k*n)的時間來遍歷陣列，若使用線段樹的話，只需要用O(k*logN)的時間，其中k為interval的個數。