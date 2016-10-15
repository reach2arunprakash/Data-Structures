# Maximum Size Subarray Sum Equals k

[Leetcode](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/)

題意：

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

Example 1:
Given nums = [1, -1, 5, -2, 3], k = 3,
return 4. (because the subarray [1, -1, 5, -2] sums to 3 and is the longest)

Example 2:
Given nums = [-2, -1, 2, 1], k = 1,
return 2. (because the subarray [-1, 2] sums to 1 and is the longest)

Follow Up:
Can you do it in O(n) time?

解題思路：

通常不能排序加上需要O(N)的話，需要額外的空間來幫助我們簡化。

我們使用一個hash map，key為nums[0] - nums[i] 的總和，i 為最一開始的i，一但map裡面有key的話，就不要再變動了。

程式碼如下：

```java
public class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        Map<Integer, Integer> map = new HashMap<>();
        int maxLength = 0;
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (sum == k) {
                maxLength = i + 1;
            } else if (map.containsKey(sum - k)) {
                maxLength = Math.max(maxLength, i - map.get(sum - k));
            }
            if (!map.containsKey(sum)) {
                map.put(sum, i);
            }
        }
        
        return maxLength;
    }
}
```
