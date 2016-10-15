# Minimum Path Sum

[Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

題意：

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

解題思路：

使用dp加滾動陣列來作，程式碼如下：

>dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]

```java
public class Solution {
    public int minPathSum(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        
        int rows = grid.length;
        int cols = grid[0].length;
        int[] dp = new int[cols];
        dp[0] = grid[0][0];
        for (int i = 1; i < cols; i++) {
            dp[i] = dp[i - 1] + grid[0][i];
        }
        
        for (int i = 1; i < rows; i++) {
            dp[0] = dp[0] + grid[i][0];
            for (int j = 1; j < cols; j++) {
                dp[j] = Math.min(dp[j], dp[j - 1]) + grid[i][j];
            }
        }
        
        return dp[cols - 1];
    }
}
```