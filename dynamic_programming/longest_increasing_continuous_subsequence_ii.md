#Longest Increasing Continuous subsequence II

[](http://www.lintcode.com/en/problem/longest-increasing-continuous-subsequence-ii/)

題意：給予一個二維陣列，在陣列中找出最長連續遞增序列，可以往上下左右的方向走。

Give you an integer matrix (with row size n, column size m)，find the longest increasing continuous subsequence in this matrix. (The definition of the longest increasing continuous subsequence here can start at any row or column and go up/down/right/left any direction).

Example
Given a matrix:
```
[1 ,2 ,3 ,4 ,5],
[16,17,24,23,6],
[15,18,25,22,7],
[14,19,20,21,8],
[13,12,11,10,9]
```
return 25

解題思路：可以利用類似深度優先搜尋來往下探索找出以當前元素起始的最長連續遞增序列，再透過記憶化搜尋模版來幫助我們避免重複計算來達到節省空間複雜度，原始碼如下：

```java
public class Solution {
    /**
     * @param A an integer matrix
     * @return  an integer
     */
    int[][] dp;
    boolean[][] visited;
    int[] dx = {1, -1, 0 , 0}; 
    int[] dy = {0, 0, 1, -1}; 
    int rows;
    int cols;
    public int longestIncreasingContinuousSubsequenceII(int[][] A) {
        
        if (A == null || A.length == 0) {
            return 0;
        }
        
        rows = A.length;
        cols = A[0].length;
        int maxLength = 1;
        dp = new int[rows][cols];
        visited = new boolean[rows][cols];
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                maxLength = Math.max(maxLength, memorySearch(A, i, j));
            }
        }
        
        return maxLength;
    }
    
    private int memorySearch(int[][] A, int x, int y) {
        
        if (visited[x][y] == true) {
            return dp[x][y];
        }
        
        // 不斷探索當前元素的上下左右元素，若有比它大，則繼續往下搜尋，
        // 搜尋完回傳的值則為以newX與newY出發能走的最長連續增增序列
        int res = 1;
        for (int i = 0; i < dx.length; i++) {
            for (int j = 0; j < dy.length; j++) {
                int newX = x + dx[i];
                int newY = y + dy[i];
                if (newX >= 0 && newX < rows && newY >= 0 && newY < cols) {
                    if (A[x][y] < A[newX][newY]) {
                        res = Math.max(res, memorySearch(A, newX, newY) + 1);
                    }
                }
            }
        }
        visited[x][y] = true;
        dp[x][y] = res;
        return res;
    }
}

```
>Time Complexity：$$O(N x M)

---
###Reference
1. http://www.jiuzhang.com/solutions/longest-increasing-continuous-subsequence-ii/