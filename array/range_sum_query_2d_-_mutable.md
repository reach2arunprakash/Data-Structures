# Range Sum Query 2D - Mutable

[Leetcode](https://leetcode.com/problems/range-sum-query-2d-mutable/)

**題意：**

*Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).*

**Example:**
```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
update(3, 2, 2)
sumRegion(2, 1, 4, 3) -> 10
```
**Note:**

1. The matrix is only modifiable by the update function.
2. You may assume the number of calls to update and sumRegion function is distributed evenly.
3. You may assume that row1 ≤ row2 and col1 ≤ col2.

**解題思路：**

bit實作請參考以下精美的解說 [Link](https://www.topcoder.com/community/data-science/data-science-tutorials/binary-indexed-trees/#2d)
一樣是使用bit來解這道題，只是換成二維的方式來解決。

需注意的地方是因為二維的部份值會重複減掉，需要把多減的加回來。

>getNum(row2, col2) - getNum(row1 - 1, col2) - getNum(row2, col1 - 1) + getNum(row1 - 1, col1 - 1);

```java
public class NumMatrix {
    int[][] matrix;
    int[][] BIT;
    int rows;
    int cols;
    public NumMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0) {
            return;
        }
        
        this.matrix = matrix;
        rows = matrix.length;
        cols = matrix[0].length;
        BIT = new int[rows + 1][cols + 1];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                init(i, j, matrix[i][j]);
            }
        }
    }
    
    public void init (int row, int col, int val) {
        row++;
        col++;
        while (row <= rows) {
            int y1 = col;
            while (y1 <= cols) {
                BIT[row][y1] += val;
                y1 += (y1 & -y1);
            }
            row += (row & -row);
        }
    }
    public void update(int row, int col, int val) {
        int delta = val - matrix[row][col];
        matrix[row][col] = val;
        init(row, col, delta);
    }
    
    public int getNum(int row, int col) {
        int sum = 0;
        row++;
        col++;
        while (row > 0) {
            int y = col;
            while (y > 0) {
                sum += BIT[row][y];
                y -= (y & -y);
            }
            row -= (row & -row);
        }
        
        return sum;
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return getNum(row2, col2) - getNum(row1 - 1, col2) - getNum(row2, col1 - 1) + getNum(row1 - 1, col1 - 1);
    }
}


// Your NumMatrix object will be instantiated and called as such:
// NumMatrix numMatrix = new NumMatrix(matrix);
// numMatrix.sumRegion(0, 1, 2, 3);
// numMatrix.update(1, 1, 10);
// numMatrix.sumRegion(1, 2, 3, 4);
```
---
###Reference
1. https://leetcode.com/discuss/72685/share-my-java-2-d-binary-indexed-tree-solution