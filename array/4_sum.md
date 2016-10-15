# 4 Sum

[Leetcode](https://leetcode.com/problems/4sum/)

題意：

Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note:
Elements in a quadruplet (a,b,c,d) must be in non-descending order. (ie, a ≦ b ≦ c ≦ d)
The solution set must not contain duplicate quadruplets.
    For example, given array S = {1 0 -1 0 -2 2}, and target = 0.
```
    A solution set is:
    (-1,  0, 0, 1)
    (-2, -1, 1, 2)
    (-2,  0, 0, 2)
```

解題思路：

用3sum再加一層，SUM == target 的條件要放在前面。

```java
public class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> res = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 3; i++) {
            if (i != 0 && nums[i - 1] == nums[i]) {
                continue;
            }
            
            for (int j = i + 1; j < nums.length - 2; j++) {
                if (j != i + 1 && nums[j - 1] == nums[j]) {
                    continue;
                }
                
                int left = j + 1;
                int right = nums.length - 1;
                while (left < right) {
                    int sum = nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum == target) {
                        List<Integer> tempRes = new ArrayList<>();
                        tempRes.add(nums[i]);
                        tempRes.add(nums[j]);
                        tempRes.add(nums[left]);
                        tempRes.add(nums[right]);
                        res.add(tempRes);
                        left++;
                        right--;
                        
                        while (left < right && nums[left - 1] == nums[left]) {
                            left++;
                        }
                        
                        while (left < right && nums[right + 1] == nums[right]) {
                            right--;
                        }
                    } else if (sum > target) {
                        right--;
                    } else {
                        left++;   
                    }
                }
            }
            
        }
        
        return res;
    }
}
```