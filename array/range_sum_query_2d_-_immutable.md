# Range Sum Query 2D - Immutable

[Leetcode](https://leetcode.com/problems/range-sum-query-2d-immutable/)

題意：

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

Example:

```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]
```
- sumRegion(2, 1, 4, 3) -> 8
- sumRegion(1, 1, 2, 2) -> 11
- sumRegion(1, 2, 2, 4) -> 12

**Note:**
You may assume that the matrix does not change.
There are many calls to sumRegion function.

解題思路：

建立一個sum 的二維陣列

>sum[i][j] 表示從matrix[0][0] - matrix[i][j] 的submatrix元素總和，接著只需要利用subarray sum的方式

sum[i] - sum[j] 表示sum[i到j]的元素總和

```java
public class NumMatrix {
    int[][] sum;
    public NumMatrix(int[][] matrix) {
        
        if (matrix == null || matrix.length == 0) {
            return;
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        sum = new int[rows + 1][cols + 1];
        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                sum[i][j] = sum[i - 1][j] + sum[i][j - 1] + matrix[i - 1][j - 1] - sum[i - 1][j - 1];
            }
        }
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return sum[row2 + 1][col2 + 1] - sum[row1][col2 + 1] - sum[row2 + 1][col1] + sum[row1][col1];
    }
}


// Your NumMatrix object will be instantiated and called as such:
// NumMatrix numMatrix = new NumMatrix(matrix);
// numMatrix.sumRegion(0, 1, 2, 3);
// numMatrix.sumRegion(1, 2, 3, 4);
```

---
###Reference
1. https://leetcode.com/discuss/69424/clean-c-solution-and-explaination-o-mn-space-with-o-1-time