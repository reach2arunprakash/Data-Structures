#Minimum Size Subarray Sum

[原題網址](http://www.lintcode.com/en/problem/minimum-size-subarray-sum/)

題意，給一個陣列與一個目標值，找出最少連續元素個數總和大於目標值。

解題思路：

可用暴力法枚舉所有可能性但需花 $$O(N^2)$$ 的時間複雜度，且含有許多重複運算的動作。

另外使用兩個指針方法來優化，一開始固定 i ，當未滿足條件時，不斷的移動 j ，一但滿足了條件，再慢慢移動 i 去縮短路徑長度。


```java
public int minimumSize(int[] nums, int s) {
    
    if (nums == null || nums.length == 0) {
        return -1;
    }
    
    int sum = 0;
    int min = Integer.MAX_VALUE;
    
    for (int i = 0, j = 0; i < nums.length; i++) {

        while (j < nums.length && sum < s) {
            sum += nums[j];
            j++;
        }
        
        if (sum >= s) {
            min = Math.min(min, j - i);
        }
        
        sum -= nums[i];
    }
    
    if (min == Integer.MAX_VALUE) {
        return -1;
    } else {
        return min;
    }
    
}
```
>Time Complexity：$$O(2N) = O(N)$$，因 i 與 j 各需移動N次

網友 [Grandyang]() 提供了以下 O(N) 解法的思路：

"這個解法要用到二分查找法，思路是，我們建立一個比原數組長一位的sums數組，其中sums[i]表示nums數組中[0, i - 1]的和，然後我們對於sums中每一個值sums[i]，用二分查找法找到子數組的右邊界位置，使該子數組之和大於sums[i] + s，然後我們更新最短長度的距離即可。"




---
###Reference
1. http://www.cnblogs.com/grandyang/p/4501934.html
