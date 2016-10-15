#Maximal Square

[原題網址](http://www.lintcode.com/submission/1540325/)

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

暴力法：枚舉陣列中的每個元素當矩型的左上元素，不斷的向右下延伸，時間複雜度約為 $$O(N^{6})$$

動態規劃法： dp[i][j] 代表以 i 與 J 作為正方形右下角可以拓展的最大邊長，即找出相鄰的三個元素(左上，上，左)中能延伸的最短長度再加一，時間複雜度約為 $$O(N^{2})$$，空間複雜度為 $$O(N^{2})$$。

如matrix[i][j] == 1，表示正方形可以matrix[i][j]這個點作延伸，則
> 
```dp[i][j] = min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]) + 1```

如matrix[i][j] == 0，表示正方形無法matrix[i][j]這個點作延伸，則
>```dp[i][j] = 0```

由於計算 dp[i][j] 時只需 要dp[i] 與 dp[i - 1] 行，因此可利用滾動陣列來將空間進而優化到 $$O(N)$$，狀態轉換如下：

>```dp[i % 2][j] = min(dp[(i - 1) % 2][j], dp[i % 2][j - 1], dp[(i - 1) % 2][j - 1]) + 1```


```java
public int maxSquare(int[][] matrix) {
    
    if (matrix == null || matrix.length == 0) {
        return 0;
    }
    
    int length = matrix.length;
    int width = matrix[0].length;
    int maxLength = 0;
    int[][] dp = new int[length][width];
    
    for (int i = 0; i < length; i++) {
        dp[i][0] = matrix[i][0];
        if (dp[i][0] == 1) {
            maxLength = 1;
        }
    }
    
    for (int i = 0; i < width; i++) {
        dp[0][i] = matrix[0][i];
        if (dp[0][i] == 1) {
            maxLength = 1;
        }
    }

    for (int i = 1; i < length; i++) {
        for (int j = 1; j < width; j++) {
            if (matrix[i][j] == 0) {
                dp[i][j] = 0;
            } else {
                dp[i][j] = Math.min(dp[i - 1][j], Math.min(dp[i - 1][j - 1], dp[i][j - 1])) + 1;
                maxLength = (dp[i][j] > maxLength) ? dp[i][j] : maxLength;
            }
        }
    }
    
    return maxLength * maxLength;
}
```
---
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

---
###Reference
1. http://www.cnblogs.com/lichen782/p/leetcode_maximal_rectangle.html#3201418