#Trapping Rain Water

[Leetcode](https://leetcode.com/problems/trapping-rain-water/)

題意：Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example, 
Given [0,1,0,2,1,0,1,3,2,1,2,1], return 6.

![](http://www.leetcode.com/wp-content/uploads/2012/08/rainwatertrap.png)
解題思路：

updated on 2016.1.18

```java
public class Solution {
    public int trap(int[] height) {
        int left = 0;
        int right = height.length - 1;
        
        int level = 0;
        int area = 0;
        while (left < right) {
            if (height[left] < height[right]) {
                level = Math.max(height[left], level);
                area += level - height[left];
                left++;
            } else {
                level = Math.max(height[right], level);
                area += level - height[right];
                right--;
            }
        }
        
        return area;
        
        
    }
}
```

此為雙指針的問題，可以利用兩個陣列left與right分別記錄當前柱子左邊最高高度與右邊最高高度，接著再透過找出當前柱子左右兩邊最高高度的較小值，並減去當前柱子高度即可得到當前柱子能灌多少水了，程式如下。

```java
public int trapRainWater(int[] heights) {
    
    if (heights == null || heights.length == 0) {
        return 0;
    }
    
    // 使用一個left陣列，來紀錄當前柱子左邊最高的柱子高度
    int[] left = new int[heights.length];
    int max = heights[0];
    left[0] = heights[0];
    
    // 從左往右掃一遍，對於每個heights[i]，找出他左邊最大的高度並存入left[i]
    for (int i = 1; i < heights.length; i++) {
        if (max < heights[i]) {
            max = heights[i];
            left[i] = max;
        } else {
            left[i] = max;
        }
    }
    
    // 使用一個right陣列，來紀錄當前柱子右邊最高的柱子高度
    int[] right = new int[heights.length];
    max = heights[heights.length - 1];
    right[heights.length - 1] = heights[heights.length - 1];
    
    // 從右往左掃一遍，對於每個heights[i]，找出他右邊最大的高度並存入right[i]
    for(int i = heights.length - 2; i >= 0; i--) {
        if (max < heights[i]) {
            max = heights[i];
            right[i] = max;
        } else {
            right[i] = max;
        }
    }
    
    // 計算總容積，從1到 到數第二個，因第一個柱子與最後一個柱子一定沒辦法灌水
    int volume = 0;
    for (int i = 1; i < heights.length - 1; i++) {
        // 找出該柱子的左右兩邊較小的柱子，並減掉當前柱子的容積，
        // 便為目前柱子能灌多少水，若當前容積比0大代表可灌水，則加入volume。
        int curVolume = Math.min(right[i], left[i]) - heights[i];
        if (curVolume > 0) {
            volume += curVolume;
        }
    }
    
    return volume;
}
```

>Time Complexity：$$O(N)$$，Space Complexity：$$O(N)$$

---
###Reference
1. https://leetcode.com/discuss/63606/very-concise-java-solution-no-stack-with-explanations