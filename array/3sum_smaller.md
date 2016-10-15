# 3Sum Smaller

[Leetcode](https://leetcode.com/problems/3sum-smaller/)

題意：

Given an array of n integers nums and a target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy the condition nums[i] + nums[j] + nums[k] < target.

For example, given ```nums = [-2, 0, 1, 3]```, and target = 2.

Return 2. Because there are two triplets which sums are less than 2:
```
[-2, 0, 1]
[-2, 0, 3]
```
Follow up:
Could you solve it in O(n2) runtime?


解題思路：

此題題意沒說清楚，重複的解答也要算在進去。

暴力法就花O(N^3)全部比對一遍

O(N^2)法就是先把陣列排序後，用3sum的方法來算，特別的地方如下

>如果sum >= target 則 j--

>如果sum < target 那麼 j - i 都是解

程式碼如下：

```java
public class Solution {
    
    public int threeSumSmaller(int[] nums, int target) {
        if (nums == null || nums.length < 3) {
            return 0;
        }
        
        Arrays.sort(nums);
        int count = 0;
        for (int i = 0; i < nums.length - 2; i++) {
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                // 重點在此，一旦sum小於target，則nums[right]搭配nums[left~(right-1)]這些都是解
                if (sum < target) {
                    count += right - left;
                    left++;
                } else if (sum >= target) {
                    right--;
                }
            }
        }
        
        return count;
    }
}
```

---
###Reference
1. http://meetqun.com/thread-10672-1-1.html

