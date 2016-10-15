# Sorrounded Regions

[Leetcode](https://leetcode.com/problems/surrounded-regions/)

題意：

Given a 2D board containing 'X' and 'O', capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

For example,
```
X X X X
X O O X
X X O X
X O X X
```
After running your function, the board should be:
```
X X X X
X X X X
X X X X
X O X X
```

解題思路：

[Code Ganker](http://blog.csdn.net/linhuanmars/article/details/22904855) 提供這是一個著名的 [Flood fill](https://zh.wikipedia.org/wiki/Flood_fill) 圖形演算法，其實就是從一個點出發對周圍區域進行目標顏色的填充。

另外網友 [ojshilu](http://blog.csdn.net/ojshilu/article/details/22600449)提供以下思路

首先，外圍一圈上的O肯定會保留下來。然後，從外圍的O能達到的O也要保留。剩下其他的O就是內部的O。

所以方法就是從外圍的一圈進行DFS算法：依次對外圈的「O」做DFS，將其可以到達O臨時設置為#。

特殊用例：只有外圍輪廓沒有內部。比如長或者寬小於2，此時不存在被包圍的'X'。

> 注意，直接往走最外圈，一遇到有 O 的 entry 則往該元素往內作dfs，把不該改的用 # 標示起來，剩下的皆不動，dfs皆執行完之後，再跑一個 nested for loop把 # 以外的元素皆改為 X。

```java
public class Solution {
    int rows;
    int cols;
    public void solve(char[][] board) {
        if (board == null || board.length == 0) {
            return;
        }
        rows = board.length;
        cols = board[0].length;
        
        // first row
        for (int i = 0; i < cols; i++) {
            if (board[0][i] == 'O') {
                board[0][i] = '#';
                dfs(board, 0, i);
            }
        }
        
        // last row
        for (int i = 0; i < cols; i++) {
            if (board[rows - 1][i] == 'O') {
                board[rows - 1][i] = '#';
                dfs(board, rows - 1, i);
            }
        }
        // left column
        for (int i = 1; i < rows; i++) {
            if (board[i][0] == 'O') {
                board[i][0] = '#';
                dfs(board, i, 0);
            }
        }
        
        for (int i = 1; i < rows; i++) {
            if (board[i][cols - 1] == 'O') {
                board[i][cols - 1] = '#';
                dfs(board, i, cols - 1);
            }
        }
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (board[i][j] == '#') {
                    board[i][j] = 'O';
                } else {
                    board[i][j] = 'X';
                }
            }
        }
    }
    
    public void dfs(char[][] board, int x, int y) {
        int[] idx = new int[]{-1, 1, 0, 0};
        int[] idy = new int[]{0, 0, -1, 1};
        
        for (int i = 0; i < idx.length; i++) {
            int newX = x + idx[i];
            int newY = y + idy[i];
            
            // 排除最外面那一圈
            if (newX > 0 && newX < rows - 1 && newY > 0 && newY < cols - 1) {
                if (board[newX][newY] == 'O') {
                    board[newX][newY] = '#';
                    dfs(board, newX, newY);
                }
            }
        }
    }
}
```

---
###Reference
1. http://blog.csdn.net/linhuanmars/article/details/22904855
2. http://blog.csdn.net/ojshilu/article/details/22600449
