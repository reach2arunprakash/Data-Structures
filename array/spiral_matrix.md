#Spiral Matrix

[Leetcode](https://leetcode.com/problems/spiral-matrix/)

題意：

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

For example,
Given the following matrix:
```
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
```
You should return

```
[1,2,3,6,9,8,7,4,5]
```



解題思路：

設(上，下，左，右)四個邊際值，不斷的以右，下，左，上的順序走訪陣列，記得檢查是否交錯了，若是則直接中斷。

```java
public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<Integer>();
        if (matrix == null || matrix.length == 0) {
            return res;
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        int left = 0; // upper start
        int right = cols - 1; // upper end
        int top = 0;
        int bottom = rows - 1;
        
        while (true) {
            for (int i = left; i <= right; i++) {
                res.add(matrix[top][i]);
            }
            top++;
            if (isCrossed(left, right, top, bottom)) {
                break;
            }
            
            for (int i = top; i <= bottom; i++) {
                res.add(matrix[i][right]);
            }
            right--;
            if (isCrossed(left, right, top, bottom)) {
                break;
            }
            
            for (int i = right; i >= left; i--) {
                res.add(matrix[bottom][i]);
            }
            bottom--;
            if (isCrossed(left, right, top, bottom)) {
                break;
            }
            
            for (int i = bottom; i >= top; i--) {
                res.add(matrix[i][left]);
            }
            left++;
            if (isCrossed(left, right, top, bottom)) {
                break;
            }
        }
        return res;
    }
    
    
    private boolean isCrossed(int left, int right, int top, int bottom) {
        if (left > right || top > bottom) {
            return true;
        }
        return false;
    }
    
}
```