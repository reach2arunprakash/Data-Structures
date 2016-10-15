# Range Sum Query - Mutable

[Leetcode](https://leetcode.com/problems/range-sum-query-mutable/)

**題意：**

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.
Example:
Given nums = [1, 3, 5]
```
sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```
**Note:**
The array is only modifiable by the update function.
You may assume the number of calls to update and sumRange function is distributed evenly.



**解題思路：**

updated on 2016.1.3

實作線段樹需要太多程式碼，網友 [rikimberley](https://leetcode.com/discuss/74222/java-using-binary-indexed-tree-with-clear-explanation)提供的神奇簡單作法：

>詳細怎麼實作的可以參考以下，太詳細了[連結](https://www.topcoder.com/community/data-science/data-science-tutorials/binary-indexed-trees/)

> i -= (i & -i) 表示是要把數中二元表示法的最右邊的去掉，可以節省很多時間，不需要shift 32次。如果是long可以節省更多時間。

>updated與init時，會影響到的只是index後面的數，因此我們需要用加的方式不斷到最後BIT[INDEX]

>如果是getnum的話，我們要不斷把前面相關的所有結果全加進來，因此使用減的方式來掃到前面的所有BIT[INDEX]

```java
public class NumArray {
    /**
     * Binary Indexed Trees (BIT or Fenwick tree):
     * https://www.topcoder.com/community/data-science/data-science-
     * tutorials/binary-indexed-trees/
     * 
     * Example: given an array a[0]...a[7], we use a array BIT[9] to
     * represent a tree, where index [2] is the parent of [1] and [3], [6]
     * is the parent of [5] and [7], [4] is the parent of [2] and [6], and
     * [8] is the parent of [4]. I.e.,
     * 
     * BIT[] as a binary tree:
     *            ______________*
     *            ______*
     *            __*     __*
     *            *   *   *   *
     * indices: 0 1 2 3 4 5 6 7 8
     * 
     * BIT[i] = ([i] is a left child) ? the partial sum from its left most
     * descendant to itself : the partial sum from its parent (exclusive) to
     * itself. (check the range of "__").
     * 
     * Eg. BIT[1]=a[0], BIT[2]=a[1]+BIT[1]=a[1]+a[0], BIT[3]=a[2],
     * BIT[4]=a[3]+BIT[3]+BIT[2]=a[3]+a[2]+a[1]+a[0],
     * BIT[6]=a[5]+BIT[5]=a[5]+a[4],
     * BIT[8]=a[7]+BIT[7]+BIT[6]+BIT[4]=a[7]+a[6]+...+a[0], ...
     * 
     * Thus, to update a[1]=BIT[2], we shall update BIT[2], BIT[4], BIT[8],
     * i.e., for current [i], the next update [j] is j=i+(i&-i) //double the
     * last 1-bit from [i].
     * 
     * Similarly, to get the partial sum up to a[6]=BIT[7], we shall get the
     * sum of BIT[7], BIT[6], BIT[4], i.e., for current [i], the next
     * summand [j] is j=i-(i&-i) // delete the last 1-bit from [i].
     * 
     * To obtain the original value of a[7] (corresponding to index [8] of
     * BIT), we have to subtract BIT[7], BIT[6], BIT[4] from BIT[8], i.e.,
     * starting from [idx-1], for current [i], the next subtrahend [j] is
     * j=i-(i&-i), up to j==idx-(idx&-idx) exclusive. (However, a quicker
     * way but using extra space is to store the original array.)
     */

    int[] nums;
    int[] BIT;
    int n;

    public NumArray(int[] nums) {
        this.nums = nums;

        n = nums.length;
        BIT = new int[n + 1];
        for (int i = 0; i < n; i++)
            init(i, nums[i]);
    }

    public void init(int i, int val) {
        i++;
        while (i <= n) {
            BIT[i] += val;
            i += (i & -i);
        }
    }

    void update(int i, int val) {
        int diff = val - nums[i];
        nums[i] = val;
        init(i, diff);
    }

    public int getSum(int i) {
        int sum = 0;
        i++;
        while (i > 0) {
            sum += BIT[i];
            i -= (i & -i);
        }
        return sum;
    }

    public int sumRange(int i, int j) {
        return getSum(j) - getSum(i - 1);
    }
}

```

一樣是使用線段樹來幫忙解決這問題，因為他會不斷的修改，線段樹可達到O(logN)的RMQ複雜度。

```java
public class NumArray {
    SegmentTree segmentTree;
    public NumArray(int[] nums) {
        segmentTree = new SegmentTree(nums);
        
    }

    void update(int i, int val) {
        segmentTree.update(i, val);
    }

    public int sumRange(int i, int j) {
        return segmentTree.sumRange(i, j);
    }
}

class SegmentTreeNode {
    int start;
    int end;
    int sum;
    SegmentTreeNode left;
    SegmentTreeNode right;
    public SegmentTreeNode(int start, int end) {
        this.start = start;
        this.end = end;
        this.sum = 0;
        left = null;
        right = null;
    }
}

class SegmentTree {
    SegmentTreeNode root;
    
    public SegmentTree(int[] nums) {
        root = buildST(nums, 0, nums.length - 1);
    }
    
    public SegmentTreeNode buildST(int[] nums, int start, int end) {
        if (start > end) {
            return null;
        }
        
        SegmentTreeNode root = new SegmentTreeNode(start, end);
        if (start == end) {
            root.sum = nums[start];
            return root;
        }
        
        int mid = start + (end - start) / 2;
        root.left = buildST(nums, start, mid);
        root.right = buildST(nums, mid + 1, end);
        
        root.sum = root.left.sum + root.right.sum;
        
        return root;
    }
    
    public void update (int pos, int val) {
        update(root, pos, val);
    }
    
    public void update(SegmentTreeNode root, int pos, int val) {
        if (root.start == root.end) {
            root.sum = val;
            return;
        }
        
        int mid = root.start + (root.end - root.start) / 2;
        if (pos <= mid) {
            update(root.left, pos, val);
        } else {
            update(root.right, pos, val);
        }
        
        root.sum = root.left.sum + root.right.sum;
    }
    
    public int sumRange(int start, int end) {
        return sumRange(root, start, end);
    }
    
    public int sumRange(SegmentTreeNode root, int start, int end) {
        if (root.start == start && root.end == end) {
            return root.sum;
        }
        
        int mid = root.start + (root.end - root.start) / 2;
        if (start >= mid + 1) {
            return sumRange(root.right, start, end);
        } else if (end <= mid) {
            return sumRange(root.left, start, end);
        }
        
        return sumRange(root.left, start, mid) + sumRange(root.right, mid + 1, end);
    }
}





// Your NumArray object will be instantiated and called as such:
// NumArray numArray = new NumArray(nums);
// numArray.sumRange(0, 1);
// numArray.update(1, 10);
// numArray.sumRange(1, 2);
```



---
###Reference
1. https://leetcode.com/discuss/74222/java-using-binary-indexed-tree-with-clear-explanation