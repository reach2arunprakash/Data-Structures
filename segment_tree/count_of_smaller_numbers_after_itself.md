# Count of Smaller Numbers After Itself

[Leetcode](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

題意：

You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].

Example:

Given nums = [5, 2, 6, 1]
```
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```
Return the array [2, 1, 1, 0].

解題思路：

也可以用 [Count of Smaller Numbers Before Itself]() 那個解法，只是從陣列的後面往前面加到線段樹，不過代碼量非常大，也容易出錯。

下面也是使用線段數來幫忙處理，但這裡參考到網友更精簡的解法，程式碼如下：

```java
public class Solution {
    
    public class SegmentTreeNode {
        SegmentTreeNode left;
        SegmentTreeNode right;
        int val = 0;
        int sum = 0;
        int dup = 1;
        public SegmentTreeNode (int val, int sum) {
            this.val = val;
            this.sum = sum;
        }
    }
    
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> res = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        
        int len = nums.length;
        Integer[] ans = new Integer[len];
        SegmentTreeNode root = null;
        
        for (int i = len - 1; i >= 0; i--) {
            root = insert(nums[i], root, ans, i, 0);
        }
        
        return Arrays.asList(ans);
    }
    
    private SegmentTreeNode insert(int num, SegmentTreeNode node, Integer[] ans, int i, int preSum) {
        if (node == null) {
            node = new SegmentTreeNode(num, 0);
            ans[i] = preSum;
        } else if (node.val == num) {
            node.dup++;
            ans[i] = preSum + node.sum;
        } else if (node.val > num) {
            node.sum++;
            node.left = insert(num, node.left, ans, i, preSum);
        } else {
            node.right = insert(num, node.right, ans, i, preSum + node.dup + node.sum);
        }
        
        return node;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/73762/9ms-short-java-bst-solution-get-answer-when-building-bst