#Submatrix Sum

[原題網址](http://www.lintcode.com/en/problem/submatrix-sum/)

題意：給一個二維矩陣，找出一個子矩陣的和為 0 。

解題思路：

暴力法：需耗費 $$O(N^{4})$$ 的時間來枚舉矩陣的上下左右範圍，再來將範圍內的所有元素加總。

我們可以透過枚舉 row 並搭配 Subarray Sum O(N) 的方法來達到 $$O(N^{3})$$ 的時間複雜度改良，其代碼如下：

```java
public class Solution {
    /**
     * @param matrix an integer matrix
     * @return the coordinate of the left-up and right-down number
     */
    public int[][] submatrixSum(int[][] matrix) {
        
        int[][] res = new int[2][2];
        
        if (matrix == null || matrix.length == 0) {
            return res;
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[] sum = new int[cols];
        
        for (int top = 0; top < rows; top++) {
            HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
            for (int bottom = top; bottom < rows; bottom++) {
                
                for(int k = 0; k < cols; k++) {
                    sum[k] += matrix[bottom][k];
                }
                
                int lastSum = 0;
                map.put(0, -1);
                for (int v = 0; v < cols; v++) {
                    lastSum += sum[v];
                    if (map.containsKey(lastSum)) {
                        res[0][0] = top;
                        res[0][1] = map.get(lastSum) + 1;
                        res[1][0] = bottom;
                        res[1][1] = v;
                        
                        return res;
                    }
                    
                    map.put(lastSum, v);
                }
            }
            
            for (int k = 0; k < cols; k++) {
                sum[k] -= matrix[top][k];
            }
            
        }
        
        return res;
    }
}

```
---
###Reference
1. http://www.jiuzhang.com/solutions/submatrix-sum/
2. http://techinpad.blogspot.com/2015/06/lintcode-submatrix-sum.html