# Find Minimum in Rotated Sorted Array II

[Leetcode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

題意：

>Follow up for "Find Minimum in Rotated Sorted Array":
What if duplicates are allowed?

>Would this affect the run-time complexity? How and why?
Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

The array may contain duplicates.


解題思路：


```java
public class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] < nums[right]) {
                right = mid;
            } else if (nums[mid] > nums[right]) {
                left = mid;
            } else {
                right--;
            }
        }
        
        return Math.min(nums[left], nums[right]);
    }
}
```