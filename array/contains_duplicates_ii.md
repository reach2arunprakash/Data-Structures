# Contains Duplicates II

[Leetcode](https://leetcode.com/problems/contains-duplicate-ii/)


題意：

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that **nums[i] = nums[j]** and the difference between i and j is at most k.

解題思路：

一樣用set，一但i大於k時，每加一個前要先刪掉一個set中最前面的元素。

```java
public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        if (nums == null || nums.length == 0 || k == 0) {
            return false;
        }
        
        HashSet<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < nums.length; i++) {
            if ( i > k) {
                set.remove(nums[i - k - 1]);
            }
            if (!set.add(nums[i])) {
                return true;
            }
        }
        
        return false;
    }
}
```