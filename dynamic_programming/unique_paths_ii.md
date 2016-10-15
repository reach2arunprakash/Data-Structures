# Unique Paths II

[Leetcode](https://leetcode.com/problems/unique-paths-ii/)

題意：

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

For example,

There is one obstacle in the middle of a 3x3 grid as illustrated below.
```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```
The total number of unique paths is 2.

解題思路：

使用dp來作，時間複雜度為O(N^2)，空間複雜度為O(N^2)。


```java
public class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        if (obstacleGrid == null || obstacleGrid.length == 0) {
            return 0;
        }
        
        int rows = obstacleGrid.length;
        int cols = obstacleGrid[0].length;
        int[][] res = new int[rows][cols];
        if (obstacleGrid[0][0] == 1) {
            res[0][0] = 0;
        } else {
            res[0][0] = 1;
        }
        
        for (int i = 1; i < cols; i++) {
            if (obstacleGrid[0][i] != 1) {
                res[0][i] = res[0][i - 1];
            } else {
                res[0][i] = 0;
            }
        }
        
        for (int i = 1; i < rows; i++) {
            if (obstacleGrid[i][0] != 1) {
                res[i][0] = res[i - 1][0];
            } else {
                res[i][0] = 0;
            }
        }
        
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {
                if (obstacleGrid[i][j] == 0) {
                    res[i][j] = res[i - 1][j] + res[i][j - 1];
                } else {
                    res[i][j] = 0;
                }
            }
        }
        
        return res[rows - 1][cols - 1];
    }
}
```

上面的程式碼太雜了，有進步的空間，一開始先判斷開頭是否能走，再來用一維陣列來節省空間

程式碼如下：

```java
public class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        //加了判斷a[0][0]的條件，如果一開始1的話，後面也不用算了
        if (obstacleGrid == null || obstacleGrid.length == 0 || obstacleGrid[0][0] == 1) {
            return 0;
        }
        
        int rows = obstacleGrid.length;
        int cols = obstacleGrid[0].length;
        int[] res = new int[cols];
        res[0] = 1;
        for (int j = 1; j < cols; j++) {
            if (obstacleGrid[0][j] == 1) {
                res[j] = 0;
            } else {
                res[j] = res[j - 1];
            }
        }
        for (int i = 1; i < rows; i++) {
            // 判斷目前i,0是否有路走，如果有路走，則直接抄下res[0]，res[0]代表res[i - 1][0]
            res[0] = obstacleGrid[i][0] == 1 ? 0 : res[0];
            for (int j = 1; j < cols; j++) {
                if (obstacleGrid[i][j] == 0) {
                    res[j] = res[j] + res[j - 1];
                } else {
                    res[j] = 0;
                }
            }
        }
        
        return res[cols - 1];
    }
}
```
---
###Reference
1. http://bangbingsyb.blogspot.com/2014/11/leetcode-unique-paths-i-ii.html