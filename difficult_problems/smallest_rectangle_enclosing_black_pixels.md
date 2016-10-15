# Smallest Rectangle Enclosing Black Pixels

[Leetcode](https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/)

題意：

An image is represented by a binary matrix with 0 as a white pixel and 1 as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location (x, y) of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

For example, given the following image:
```
[
  "0010",
  "0110",
  "0100"
]
```
and x = 0, y = 2,
Return 6.



解題思路：

updated on 2016.1.3

```java
public class Solution {
    public int minArea(char[][] image, int x, int y) {
        if (image == null || image.length == 0) {
            return 0;
        }
        
        int rows = image.length;
        int cols = image[0].length;
        
        int[] axis = new int[4];
        axis[0] = rows - 1; // upper
        axis[1] = 0; //bottom
        axis[2] = cols - 1; // left;
        axis[3] = 0; // right;
        
        dfs(image, x, y, axis);
        
        return (axis[1] - axis[0] + 1) * (axis[3] - axis[2] + 1);
        
    }
    
    private void dfs(char[][] image, int x, int y, int[] axis) {
        int rows = image.length;
        int cols = image[0].length;
        if (x < 0 || x >= rows || y < 0 || y >= cols || image[x][y] == '0') {
            return;
        }
        
        image[x][y] = '0';
        axis[0] = Math.min(x, axis[0]);
        axis[1] = Math.max(x, axis[1]);
        axis[2] = Math.min(y, axis[2]);
        axis[3] = Math.max(y, axis[3]);
        
        dfs(image, x - 1, y, axis);
        dfs(image, x + 1, y, axis);
        dfs(image, x, y - 1, axis);
        dfs(image, x, y + 1, axis);
    }
}
```

主要是作以下的事情

>- top = search row [0...x], find first row contain 1,
- bottom = search row[x+1, row], find first row contian all 0
- left = search col[0...y], find first col contain 1,
- right = search col[y+1, col], find first col contain all 0
- return (right - left) * (bottom - top)

DFS法：

```java
public class Solution {
    public int minArea (char[][] image, int x, int y) {
        int column = image.length; // vertical
        if (column == 0) return 0;
        int row = image[0].length; // horizontal

        int[] res = new int[4];
        res[0] = column-1; // initial upper bound value
        res[1] = 0;        // initial bottom bound value
        res[2] = row - 1;  // initial left bound value
        res[3] = 0;        // initial right bound value
        dfs(image, x, y, res);
        return (res[1]-res[0]+1) * (res[3]-res[2]+1); // (bot - upper + 1) * (right - left + 1)
    }

    public void dfs(char[][] image, int x, int y, int[] res) {
        int column = image.length;
        int row = image[0].length;
        if (x < 0 || x > column-1 || y < 0 || y > row-1) return; 
        if (image[x][y] == '0') return;
        image[x][y] = '0';          // once visit, set to 0

        if (x < res[0]) res[0] = x; // update upper bound
        if (x > res[1]) res[1] = x; // update bottom bound
        if (y < res[2]) res[2] = y; // update left bound
        if (y > res[3]) res[3] = y; // update right bound

        dfs(image, x+1, y, res);
        dfs(image, x, y+1, res);
        dfs(image, x-1, y, res);
        dfs(image, x, y-1, res);
    }
}
```

Binary Search 搜尋法：

```java
public class Solution {
    private char[][] image;
    public int minArea(char[][] iImage, int x, int y) {
        image = iImage;
        int m = image.length;
        int n = image[0].length;
        
        int left = searchColumns(0, y, 0, m, true);
        int right = searchColumns(y + 1, n, 0, m, false);
        int top = searchRows(0, x, left, right, true);
        int bottom = searchRows(x + 1, m, left, right, false);
        
        return (right - left) * (bottom - top);
    }
    
    private int searchColumns(int i, int j, int top, int bottom, boolean opt) {
        while (i != j) {
            int k  = top;
            int mid = (i + j) / 2;
            while (k < bottom && image[k][mid] == '0') {
                k++;
            }
            
            if (k < bottom == opt) {
                j = mid;
            } else {
                i = mid + 1;
            }
        }
        
        return i;
    }
    
    private int searchRows(int i, int j, int left, int right, boolean opt) {
        while (i != j) {
            int k  = left;
            int mid = (i + j ) / 2;
            while (k < right && image[mid][k] == '0') {
                k++;
            }
            if (k < right == opt) {
                j = mid;
            } else {
                i = mid + 1;
            }
        }
        
        return i;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/68246/c-java-python-binary-search-solution-with-explanation



---
###Reference
1. https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/