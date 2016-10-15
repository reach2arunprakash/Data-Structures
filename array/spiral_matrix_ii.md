# Spiral Matrix II

[Leetcode](https://leetcode.com/problems/spiral-matrix-ii/)

題意：

Given an integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.

For example,
Given n = 3,

You should return the following matrix:
```
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

解題思路：

與i完全相同，差別在於是要產生出一個Spiral Matrix，所利用一個idx直，不斷的作assign的動作即可，程式碼如下：

```java
public class Solution {
    public int[][] generateMatrix(int n) {
        int[][] res = new int[n][n];
        
        int idx = 1;
        int left = 0;
        int right = n - 1;
        int top = 0;
        int bottom = n - 1;
        
        while(true) {
            for (int i = left; i <= right; i++) {
                res[top][i] = idx;
                idx++;
            }
            top++;
            if (isValid(left, right, top, bottom)) {
                break;
            }
            
            for (int i = top; i <= bottom; i++) {
                res[i][right] = idx;
                idx++;
            }
            right--;
            if (isValid(left, right, top, bottom)) {
                break;
            }
            
            for (int i = right; i >= left; i--) {
                res[bottom][i] = idx;
                idx++;
            }
            bottom--;
            if (isValid(left, right, top, bottom)) {
                break;
            }
            
            for (int i = bottom; i >= top; i--) {
                res[i][left] = idx;
                idx++;
            }
            left++;
            if (isValid(left, right, top, bottom)) {
                break;
            }
        }
        
        return res;
    }
    
    private boolean isValid(int left, int right, int top, int bottom) {
        if (left > right || top > bottom) {
            return true;
        }
        return false;
    }
}
```