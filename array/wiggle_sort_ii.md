# Wiggle Sort II

[]()


題意：

Given an unsorted array nums, reorder it such that nums[0] < nums[1] > nums[2] < nums[3]....

Example:
1. Given nums = [1, 5, 1, 1, 6, 4], one possible answer is [1, 4, 1, 5, 1, 6]. 
2.  Given nums = [1, 3, 2, 2, 3, 1], one possible answer is [2, 3, 1, 3, 1, 2].

Note:
You may assume all input has valid answer.

Follow Up:
Can you do it in O(n) time and/or in-place with O(1) extra space?


解題思路：

O(nlogn) + O(n) 法：

先把原陣列排序，接著找出中間點，偶數index塞中間點前的數，奇數點塞中間點後的數。

```java
public class Solution {
    public void wiggleSort(int[] nums) {
        if (nums == null || nums.length < 2) {
            return;
        }
        
        int len = nums.length;
        int[] temp = new int[len];
        Arrays.sort(nums);
        System.arraycopy(nums, 0, temp, 0, nums.length);
        
        int mid = (temp.length - 1) / 2;
        
        for (int i = 0; i <= mid; i++) {
            nums[2 * i] = temp[i];
            if ((2 * i + 1) < temp.length) {
                nums[2 * i + 1] = temp[mid + 1 + i];
            }
        }
    }
}
```