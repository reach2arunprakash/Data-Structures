#Jump Game II

[原題網址](http://www.lintcode.com/en/problem/jump-game-ii/)

題意：找出最小步數到達終點

解題思路：

* Greedy 法：

在這裡是利用 curMax 來紀錄跳 k 次之後能達到的最遠距離。

> ``` d[k] = max (i + A[i])```

> ```d[k - 2] < i <= d[k - 1]```

```java
public int jump(int[] nums) {
        
    if (nums == null || nums.length == 0) {
        return -1;
    }
    
    int curMax = 0;
    int steps = 0;
    int i = 0;
    
    while (curMax < nums.length - 1) { // 表示還沒跳到
        int lastMax = curMax; // 紀錄當前的max能跳到的地方
        
        // i一旦往前走了就不回頭，
        for (; i <= lastMax; i++) {
            curMax = Math.max(curMax, i + nums[i]);
        }
        
        steps++;
        
        // 表示lastMax == curMax < n - 1
        // 則無法跳到終點，返回負一
        if (lastMax == curMax) {
            return -1;
        }
        
    }
    
    return steps;
}
```

* 
動態規劃法：


```java
public int jump(int[] A) {

    int[] result = new int[A.length];
    result[0] = 0;
    
    for (int i = 1; i < A.length; i++) {
        result[i] = Integer.MAX_VALUE;
        for (int j = 0; j < i; j++) {
            // j + A[j] >= i 為確定是否能由j跳到i，超過也沒關係
            // result[i] > result[j] + 1 為判斷目前的result是否為最小的，
            // 直接break可省時間，避免重覆計算
            if (j + A[j] >= i && result[i] > result[j] + 1) {
                result[i] = result[j] + 1;
                break;
            }
        }
    }
    return result[A.length - 1];
}
```

---
###Reference
1. http://www.cnblogs.com/lichen782/p/leetcode_Jump_Game_II.html
