# Rotate Image

[Lintcode]()

題意：

You are given an n x n 2D matrix representing an image. Rotate the image by 90 degrees (clockwise).

xample
Given a matrix
```
[
    [1,2],
    [3,4]
]
```
rotate it by 90 degrees (clockwise), return
```
[
    [3,1],
    [4,2]
]
```
Challenge
Do it in-place.

解題思路：

1. 把matrix中間水平線作反轉
```
[
    [3,4],
    [1,2]
]
```
2. 再作 matrix transpose
3. ```
[
    [3,1],
    [4,2]
]
```

程式碼如下：

```java
public class Solution {
    /**
     * @param matrix: A list of lists of integers
     * @return: Void
     */
    public void rotate(int[][] matrix) {
        
        if (matrix == null || matrix.length <= 1) {
            return;
        }
        
        int n = matrix.length;
        for (int i = 0 ; i < n / 2; i++) {
            for (int j = 0; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = temp;
            }
        }
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (i == j ) {
                    continue;
                }
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
    }
}

```

另有[網友](http://www.cnblogs.com/springfor/p/3886487.html)提供了可一次搞定的方法

```java
//in place
public void rotate(int[][] matrix) {
int n = matrix.length;
for (int i = 0; i < n / 2; i++) {
    for (int j = 0; j < Math.ceil(((double) n) / 2.); j++) {
        int temp = matrix[i][j];
        matrix[i][j] = matrix[n-1-j][i];
        matrix[n-1-j][i] = matrix[n-1-i][n-1-j];
        matrix[n-1-i][n-1-j] = matrix[j][n-1-i];
        matrix[j][n-1-i] = temp;
    }
}
```

---
###Reference
1. http://www.cnblogs.com/springfor/p/3886487.html