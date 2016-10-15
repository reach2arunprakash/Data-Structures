# Sparse Matrix Multiplication

[Leetcode](https://leetcode.com/problems/sparse-matrix-multiplication/)

題意：

Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

Example:
```
A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
```


解題思路：

暴力法，會超時：

```java
public class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        int rowA = A.length;
        int colA = A[0].length;
        int rowB = B.length;
        int colB = B[0].length;
        
        int[][] res = new int[rowA][colB];
        
        for (int i = 0; i < rowA; i++) {
            for (int j = 0; j < colB; j++) {
                for (int k = 0; k < colB; k++) {
                    res[i][j] += A[i][k] * B[k][j];
                }
            }
        }
        
        return res;
    }
}
```

因是sparse matrix，必包含非常多的0 ，因此我們用column major的一一判斷，一但A[i][k]為0 的話，後面也不需要作了，可以省去非常多的時間，其程式碼如下：

```java
public class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        int rowA = A.length;
        int colA = A[0].length;
        int rowB = B.length;
        int colB = B[0].length;
        
        int[][] res = new int[rowA][colB];
        
        for (int i = 0; i < rowA; i++) {
            for (int k = 0; k < colA; k++) {
                if (A[i][k] != 0) {
                    for (int j = 0; j < colB; j++) {
                        res[i][j] += A[i][k] * B[k][j];
                    }
                }
            }
        }
        
        return res;
    }
}
```

---
###Reference
1. https://leetcode.com/discuss/71912/easiest-java-solution