# 2 Sum

[Leetcode](https://leetcode.com/problems/two-sum/)

題意：

Given an array of integers, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2

解題思路：

使用hash table來紀錄之前拜訪過的點

```java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        HashMap<Integer, Integer> map = new  HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i])) {
                res[0] = map.get(target- nums[i]);
                res[1] = i + 1;
                return res;
            }
            
            map.put(nums[i], i + 1);
        }
        
        return res;
    }
}
```

如果已排序好，使用兩根指針即可。

```java
public class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] res = new int[]{-1, -1};
        if (numbers == null || numbers.length < 2) {
            return res;
        }
        
        int start = 0;
        int end = numbers.length - 1;
        while (start < end) {
            int sum = numbers[start] + numbers[end];
            if (sum < target) {
                start++;
            } else if (sum > target) {
                end--;
            } else {
                res[0] = start + 1;
                res[1] = end + 1;
                return res;
            }
        }
        
        return res;
    }
}
```