# Jump Game
[原題網址](http://www.lintcode.com/en/problem/jump-game/)

題意：

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

For example:

Given array ```A = [2,3,1,1,4]```

The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)

解題思路：

動態規劃法：

設一個can[] ，can[i] 代表是否能從原點跳到第 i 點，達成的條件是能找到一個 j ，使得原點能跳到第 j 點，且從第 j 點能跳超過 i 或是等於 i 。

> can[i] = can[j] && (j + A[j] > i) for j from 0 to i - 1


```java
public boolean canJump(int[] A) {
    // wirte your code here
    boolean[] can = new boolean[A.length];
    can[0] = true;

    for (int i = 1; i < A.length; i++) {
        for (int j = 0; j < i; j++) {
            if (can[j] && j + A[j] >= i) {
                can[i] = true;
                break;
            }
        }
    }
    return can[A.length - 1];
}
```

>Time Complexity： $$O(N^{2})$$

---
Greedy 法：紀錄一個當前能達到的最遠距離 max Index

1. 能跳到位置 i  的條件： i <= maxIndex
2. 一旦跳到 i ，則 maxIndex = max(maxIndex, i  + A[i])
3. 能跳到最後一個位置 n - 1 的條件是： maxIndex >= n - 1

其代碼如下：

public boolean canJump(int[] nums) {
        
    if (nums == null || nums.length == 0) {
        return false;
    }
    
    int len = nums.length;
    int maxIdx = 0;
    for (int i = 0; i < len; i++) {
        if (maxIdx < i || maxIdx >= len - 1) 
            break;
        maxIdx = Math.max(maxIdx, i + nums[i]);
    }
    
    return (maxIdx >= len - 1) ? true : false;
}



