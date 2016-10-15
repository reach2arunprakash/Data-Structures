# Dungeon Game

[]()

題意：

The demons had captured the princess (P) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (K) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (negative integers) upon entering these rooms; other rooms are either empty (0's) or contain magic orbs that increase the knight's health (positive integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.


Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.

For example, given the dungeon below, the initial health of the knight must be at least 7 if he follows the optimal path ```RIGHT-> RIGHT -> DOWN -> DOWN```.

| -2 (k) | -3 | 3 |
| -- | -- | -- |
| -5 | -10 | 1 |
| 10 | 30 | -5 |


解題思路：

這題主要是使用dp來作，我們要找出knight最少需要多少生命點數才能夠救到公主，走到最右下角時生命至少還要剩1才能救到公主，與之前不同的是，我們是要由右下往左上推導結果。

遞迴式如下：

>dp[i][j] = Max(1, Min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j])

dp[i][j]表示進入這個格子後保證knight不會死所需要的最小HP。如果一個格子的值為負，那麼進入這個格子之前knight需要有的最小HP是-dungeon[i][j] + 1.如果格子的值非負，那麼最小HP需求就是1.

其程式碼如下：

```java
public class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        
        if (dungeon == null || dungeon.length == 0) {
            return 0;
        }
        
        int rows = dungeon.length;
        int cols = dungeon[0].length;
        int[][] dp = new int[rows][cols];
        dp[rows - 1][cols - 1] = Math.max(1, -dungeon[rows - 1][cols - 1] + 1);
        for (int i = rows - 2; i >= 0; i--) {
            dp[i][cols - 1] = Math.max(1, dp[i + 1][cols - 1] - dungeon[i][cols - 1]);
        }
        
        for (int i = cols - 2; i >= 0; i--) {
            dp[rows - 1][i] = Math.max(1, dp[rows - 1][i + 1] -dungeon[rows - 1][i]);
        }
        
        for (int i = rows - 2; i >= 0; i--) {
            for (int j = cols - 2; j >= 0; j--) {
                dp[i][j] = Math.max(1, Math.min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j]);
            }
        }
        
        return dp[0][0];
    }
}
```

---
###Reference
1. http://blog.csdn.net/likecool21/article/details/42516979