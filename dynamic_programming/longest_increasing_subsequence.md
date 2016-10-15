#Longest Increasing Subsequence
[Lintcode](http://www.lintcode.com/en/problem/longest-increasing-subsequence/)

[Leetcode](https://leetcode.com/problems/longest-increasing-subsequence/)

題意：

Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,
Given ```[10, 9, 2, 5, 3, 7, 101, 18]```,
The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

Your algorithm should run in O(n^2) complexity.

**Follow up:** Could you improve it to O(n log n) time complexity?


解題思路：

不斷的拿當前這個數與之前所有的數來比，並一邊更新值，最後比較max。

```java
public int longestIncreasingSubsequence(int[] nums) {
    
    int[] result = new int[nums.length];
    int max = 0;
    
    for (int i = 0; i < nums.length; i++) {
        result[i] = 1;
        
        for (int j = 0; j < i; j++) {
            //拿之前的與目前這個比，若比較小，則看看result誰大就取誰
            if (nums[j] <= nums[i]) {
                result[i] = result[i] > result[j] + 1 ? result[i] : result[j] + 1;
            }
        }
        
        if (max < result[i]) {
            max = result[i];
        }
    }

    return max;
    
}
```
>Time Complexity：O(n^2)

網友提供的神之O(nlogn)解法：

會用 ```i = -(i + 1)```是因為以下java binary search的回傳值，如果元素存在的話，則直接回傳index，否則會回傳 ```(-(insertion point) - 1)```，這是需要注意的。

> 也就是說如果找到元素在dp陣列裡的話，則dp陣列不變，否則把該新的值插入正確的位置。index表示目前array中最後一個元素的index是什麼。

官方說明如下

>**Returns:**
index of the search key, if it is contained in the array within the specified range; otherwise, (-(insertion point) - 1). The insertion point is defined as the point at which the key would be inserted into the array: the index of the first element in the range greater than the key, or toIndex if all elements in the range are less than the specified key. Note that this guarantees that the return value will be >= 0 if and only if the key is found.

```java
public class Solution {
    public int lengthOfLIS(int[] nums) {            
        int[] dp = new int[nums.length];
        int len = 0;

        for(int x : nums) {
            int i = Arrays.binarySearch(dp, 0, len, x);
            if(i < 0) i = -(i + 1);
            dp[i] = x;
            if(i == len) len++;
        }

        return len;
    }
}
```


---
###Reference
1. https://leetcode.com/discuss/67609/short-java-solution-using-dp-o-n-log-n