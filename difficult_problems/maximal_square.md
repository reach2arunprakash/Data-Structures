#Maximal Square

[原題網址](https://leetcode.com/problems/maximal-square/)

題意：

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing all 1's and return its area.

For example, given the following matrix:

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

return 4

解題思路：

updated 2015.12.1

網友超短超神解答

```java
public class Solution {
    public int maximalSquare(char[][] a) {
        if(a.length == 0) return 0;
        int m = a.length, n = a[0].length, result = 0;
        int[][] b = new int[m+1][n+1];
        for (int i = 1 ; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if(a[i-1][j-1] == '1') {
                    b[i][j] = Math.min(Math.min(b[i][j-1] , b[i-1][j-1]), b[i-1][j]) + 1;
                    result = Math.max(b[i][j], result); // update result
                }
            }
        }
        return result*result;
    }
}
```

我們可以利用解 [Histogram]() 的方式來幫助我們解這題，首先算出一個高度的二維矩陣 

> height[i][j] 代表從第i列的第j個元素往上算最高的連續1為多少

以上題為例， height 矩陣為：

```
1 0 1 0 0
2 0 2 1 1
3 1 3 2 2
4 0 0 3 0
```

接著每一行皆可轉換成histogram那題來作。

程式碼如下(最後一個過不了)：

```java
public class Solution {
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0) {
            return 0;
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[][] height = new int[rows][cols];
        
        int maxArea = Integer.MIN_VALUE;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (matrix[i][j] == '0') {
                    height[i][j] = 0;
                } else {
                    height[i][j] = (i == 0) ? 1 : height[i-1][j] + 1;
                }
            }
        }
        
        for (int i = 0; i < rows; i++) {
            int area = largestRectangleArea(height[i]);
            if (area > maxArea) {
                maxArea = area;
            }
        }
        
        return maxArea;
        
    }
     
    public int largestRectangleArea(int[] height) {
        if (height == null || height.length == 0) {
            return 0;
        }
    
        // 以每一個點為中心，找出左右第一位比該點小的值
        // 使用 stack來輔助，若stack不為升序的話，則不斷pop，每一次pop即可得到兩個紀錄
        // 使得最上面那個點被彈出的值為被彈出的點之右邊第一個小的值。
        // 被pop那個點的下一個，則否左邊第一個比該點小的值
        // stack塞的數為index
        int max = 0;
        Stack<Integer> stack = new Stack<Integer>();
    
        for (int i = 0; i <= height.length; i++) {
            int cur = (i == height.length) ? -1 : height[i];
            // 記得while是把stack中第一個值當index來抓出height來比
            while (!stack.isEmpty() && cur < height[stack.peek()]) {
                // 直接用pop出來的index來取出高
                // 如果stack為empty，則代表該點右邊沒有最小的，即走到頭，所以設為i
                // 若不為empty，則把彈出後的stack取最上面的值去減i來得到寬
                int h = height[stack.pop()];
                int w = stack.isEmpty() ? i : i - stack.peek() - 1;
                if (h == w && h * w > max)
                    max = h * w;
            }
            stack.push(i);
        }
    
        return max;
    }
}
```


updated 2015.11.21

```java
public class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix == null || matrix.length == 0) {
            return 0;
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[][] heights = new int[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (matrix[i][j] == '1') {
                    heights[i][j] = (i == 0) ? 1 : heights[i - 1][j] + 1;
                } else {
                    heights[i][j] = 0;
                }
            }
        }
        
        int max = 0;
        for (int i = 0; i < rows; i++) {
            int curArea = maxAreaHistogram(heights[i]);
            max = Math.max(max, curArea);
        }
        
        return max;
    }
    
    private int maxAreaHistogram(int[] heights) {
        if (heights == null || heights.length == 0) {
            return 0;
        }
        
        int len = heights.length;
        Stack<Integer> s = new Stack<Integer>();
        int maxArea = 0;
        for (int i = 0; i <= len; i++) {
            int cur = (i == len) ? -1 : heights[i];
            while (!s.isEmpty() && cur < heights[s.peek()]) {
                int h = heights[s.pop()];
                int w = (s.isEmpty()) ? i : i - s.peek() - 1;
                maxArea = Math.max(maxArea, w * h);
            }
            s.push(i);
        }
        return maxArea;
    }
}
```
---
###Reference
1. http://www.cnblogs.com/lichen782/p/leetcode_maximal_rectangle.html#3201418
