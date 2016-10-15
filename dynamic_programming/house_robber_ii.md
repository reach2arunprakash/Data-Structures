# House Robber II

[Leetcode](https://leetcode.com/problems/house-robber-ii/)

題意：

與第一題相同，只是改成cycle

After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

解題思路：

其實與1相同，但是得分兩個情況來作：

- 搶第一間的話，得把最後一間去掉，去算max profit
- 搶最後一間的話，把第一間去掉，去算 max profit

最後再比較兩者哪個較大即可，注意邊際條件的處理，其程式碼如下：


```java
public class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return nums[0];
        }
        int len = nums.length;
        int robFirstHouse = rob(nums, 0, len - 1);
        int robLastHouse = rob(nums, 1, len);
        return Math.max(robFirstHouse, robLastHouse);
    }
    
    public int rob(int[] nums, int left, int right) {
        if(left >= right) return 0;

        int odd = 0;
        int even = 0;
        for (int i = left; i < right; i++) {
            if (i % 2 == 1) {
                odd = Math.max(odd + nums[i], even);
            } else {
                even = Math.max(odd, even + nums[i]);
            }
        }
        
        return Math.max(even ,odd);
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/68073/simple-java-solution
2. http://bookshadow.com/weblog/2015/05/20/leetcode-house-robber-ii/