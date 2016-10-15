# Word Search

[Lintcode](http://www.lintcode.com/en/problem/word-search/)

題意：

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

Example
Given board =
```
[
  "ABCE",
  "SFCS",
  "ADEE"
]

```
word = "ABCCED", -> returns true,

word = "SEE", -> returns true,

word = "ABCB", -> returns false.

>即給一個字串與一個棋盤，找出是否有一條path是以字串中字元順序走的。

解題思路：

使用 DFS 來幫助我們解這道題，不斷的往目前位置的上下左右探索，若有找到相同的字元，則往該位置繼續往下走，此法複雜度很高 O(MNMN)，作的優化就是一但找到一條路徑則馬上終止，程式碼如下，若有更優解再更新：

```java
public class Solution {
    /**
     * @param board: A list of lists of character
     * @param word: A string
     * @return: A boolean
     */
     
    boolean[][] visited;
    public boolean exist(char[][] board, String word) {
       
       if (board == null || board.length == 0 || word == null || word.length() == 0) {
           return false;
       }
       
       int rows = board.length;
       int cols = board[0].length;
       visited = new boolean[rows][cols];
       for (int i = 0; i < rows; i++) {
           for (int j = 0; j < cols; j++) {
               if (board[i][j] == word.charAt(0)) {
                   visited[i][j] = true;
                   if (dfs(board, word, 1, i, j)) {
                       return true;
                   }
                   visited[i][j] = false;
               }
           }
       }
       return false;
       
    }
    
    public boolean dfs(char[][] board, String word, int pos, int x, int y) {
        if (pos > word.length()) {
            return false;
        }
        if (pos == word.length()) {
            return true;
        }
        
        char c = word.charAt(pos);
        int[] xIdx = new int[] {-1, 1, 0, 0};
        int[] yIdx = new int[] {0, 0, -1, 1};
        boolean result = false;
        for (int i = 0; i < xIdx.length; i++) {
            int newX = x + xIdx[i];
            int newY = y + yIdx[i];
            if (newX >= 0 && newX < board.length && newY >= 0 && newY < board[0].length) {
                if (!visited[newX][newY] && board[newX][newY] == c) {
                    visited[newX][newY] = true;
                    if (dfs(board, word, pos + 1, newX, newY)) {
                        return true;
                    }
                    visited[newX][newY] = false;
                }
            }
        }
        return result;
    }
}

```