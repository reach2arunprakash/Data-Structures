# Range Sum Query - Immutable

[Leetcode](https://leetcode.com/problems/range-sum-query-immutable/)

題意：

Given an integer array nums, find the sum of the elements between indices i and j (i ≦ j), inclusive.

**Example:**
```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```
**Note:**
1. You may assume that the array does not change.
2. There are many calls to sumRange function.


解題思路：

維護一個len + 1的sum array，sum[i] 表示從 nums[0] 到 nums[i - 1]的總和。

```java
public class NumArray {
    
    int[] sums;
    public NumArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            return;
        }
        int len = nums.length;
        sums = new int[len + 1];
        int sum = 0;
        for (int i = 1; i <= len; i++) {
            sums[i] = sums[i - 1] + nums[i - 1];
        }
    }

    public int sumRange(int i, int j) {
        if (i < 0 || j >= sums.length) {
            return 0;
        }
        return sums[j + 1] - sums[i];
    }
}


// Your NumArray object will be instantiated and called as such:
// NumArray numArray = new NumArray(nums);
// numArray.sumRange(0, 1);
// numArray.sumRange(1, 2);
```