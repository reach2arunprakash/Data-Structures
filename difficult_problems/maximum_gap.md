# Maximum Gap

[Leetcode](https://leetcode.com/problems/maximum-gap/)

題意：

Given an unsorted array, find the maximum difference between the successive elements in its sorted form.

Return 0 if the array contains less than 2 elements.

Have you met this question in a real interview? Yes

Example

Given ```[1, 9, 2, 5]```, the sorted form of it is ```[1, 2, 5, 9]```, the maximum gap is between ```5``` and ```9 = 4```.

Note

You may assume all elements in the array are non-negative integers and fit in the 32-bit signed integer range.

Challenge
Sort is easy but will cost O(nlogn) time. Try to solve it in linear time and space.

解題思路：

暴力法花 O(NlogN) 排序後再不斷跟前面的值比對，程式碼如下：

```java
public class Solution {
    public int maximumGap(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        Arrays.sort(nums);
        int maxGap = 0;
        for (int i = 1; i < nums.length; i++) {
            if ((nums[i] - nums[i - 1]) > maxGap) {
                maxGap = nums[i] - nums[i - 1];
            }
        }
        return maxGap;
    }
}
```

O(N) 的解法，利用桶排序，以下為 leetcode官網的說明：

Suppose there are N elements and they range from A to B.

Then the maximum gap will be no smaller than ceiling[(B - A) / (N - 1)]

Let the length of a bucket to be len = ceiling[(B - A) / (N - 1)], then we will have at most num = (B - A) / len + 1 of bucket

for any number K in the array, we can easily find out which bucket it belongs by calculating loc = (K - A) / len and therefore maintain the maximum and minimum elements in each bucket.

Since the maximum difference between elements in the same buckets will be at most len - 1, so the final answer will not be taken from two elements in the same buckets.

For each non-empty buckets p, find the next non-empty buckets q, then q.min - p.max could be the potential answer to the question. Return the maximum of all those values.

網友 [Timshel](http://blog.hushlight.com/2015/04/leetcode-maximum-gap/) 提供以下思路：

1. 先找出max, min 值， 求得average gap為(max – min ) / (N – 1)
2. 再把N-2個數（去除min和max）分到 N-1個筐裡，第K個筐裡裝在 (min + (k-1)gap, min + k*gap) 範圍內最大和最小值
3. 最後掃瞄N-1個筐，每個筐最小值減去前面最大值


> 使用簡單版的基數排序，因最大的gap不可能在同一個桶子裡，只要不斷的去把當前桶子的最小值去與上一個有值的桶子之最大值相減，就可算出gap。

其程式碼如下：

```java
class Solution {
    /**
     * @param nums: an array of integers
     * @return: the maximum difference
     */
    public int maximumGap(int[] nums) {
        
        if (nums == null || nums.length < 2) {
            return 0;
        } 
        
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        for (int i : nums) {
            min = Math.min(min, i);
            max = Math.max(max, i);
        }
        
        int len = nums.length;
        int average = (max - min) / (len - 1) + 1; // + 1是取 ceiling
        int[] mins = new int[len - 1];
        int[] maxs = new int[len - 1];
        Arrays.fill(mins, Integer.MAX_VALUE);
        Arrays.fill(maxs, Integer.MIN_VALUE);
        
        for (int i : nums) {
            if (i == min || i == max) {
                continue;
            }
            int index = (i - min) / average;
            mins[index] = Math.min(i, mins[index]);
            maxs[index] = Math.max(i, maxs[index]);
        }
        
        int res = Integer.MIN_VALUE;
        int previous = min;
        for (int i = 0; i < nums.length - 1; i++) {
            if (mins[i] == Integer.MAX_VALUE && maxs[i] == Integer.MIN_VALUE) {
                continue;
            }
            res = Math.max(res, mins[i] - previous);
            // 改為當前最大的，因接下來要相減的值在下一個桶子裡，
            // 要保有排序後的連續性，直接找最大的，去與下一個桶子裡最小的相減
            previous = maxs[i]; 
        }
        
        res = Math.max(res, max - previous);
        return res;
        
    }
}

```

---
###Reference
1. http://www.cnblogs.com/higerzhang/p/4176108.html
2. http://www.programcreek.com/2014/03/leetcode-maximum-gap-java/
3. http://blog.hushlight.com/2015/04/leetcode-maximum-gap/