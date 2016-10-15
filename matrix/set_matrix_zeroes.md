#Set Matrix Zeroes

[原題網址](http://www.lintcode.com/en/problem/set-matrix-zeroes/)

題意：


解題思路：如果直接在原矩陣上把為0的元素之該行該列設為0 ，到最後整個矩陣都會為0 ，我們利用一個 list 來存放為0 的元素，接著再遍歷一次該 list，把list中所有元素的該行與該列設為0即可，程式碼如下：

```java
public class Solution {
    /**
     * @param matrix: A list of lists of integers
     * @return: Void
     */
     
    public class Pair {
        int x;
        int y;
        public Pair(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
    
    public void setZeroes(int[][] matrix) {
        
        if (matrix == null || matrix.length == 0) {
            return;
        }
        
        ArrayList<Pair> pairs = new ArrayList<Pair>();
        int rows = matrix.length;
        int cols = matrix[0].length;
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (matrix[i][j] == 0) {
                    pairs.add(new Pair(i, j));
                }
            }
        }
        
        for (int i = 0; i < pairs.size(); i++) {
            Pair p = pairs.get(i);
            for (int j = 0; j < rows; j++) {
                matrix[j][p.y] = 0;
            }
            for (int j = 0; j < cols; j++) {
                matrix[p.x][j] = 0;
            }
        }
    }
}
```
> Time Complexity：$$O(N^{2})$$
> Space Complexity：$$O(2M)$$，M代表元素為 的個數。

---
