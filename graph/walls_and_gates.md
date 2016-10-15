# Walls and Gates

[Leetcode](https://leetcode.com/problems/walls-and-gates/)

題意：

You are given a m x n 2D grid initialized with these three possible values.

1. -1 - A wall or an obstacle.
2. 0 - A gate.
3. INF - Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.
Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

For example, given the 2D grid:
```
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
  ```
After running your function, the 2D grid should be:
```
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
  ```


解題思路：

DFS去作，找到為0(即為gate)的點往下作dfs，只要該點的鄰居不超過範圍，且distance比當前節點的distance還大的話，則繼續再往該鄰居下去作dfs，程式碼如下：

```java
public class Solution {
    public void wallsAndGates(int[][] rooms) {
        if (rooms == null || rooms.length == 0) {
            return;
        }
        
        int rows = rooms.length;
        int cols = rooms[0].length;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (rooms[i][j] == 0) {
                    dfs(rooms, rows, cols, i, j);
                }
            }
        }
    }
    
    private void dfs(int[][] rooms, int rows, int cols, int x, int y) {
        int distance = rooms[x][y];
        
        int[] idx = new int[]{0, 0, 1, -1};
        int[] idy = new int[]{1, -1, 0, 0};
        for (int i = 0; i < 4; i++) {
            int newX = x + idx[i];
            int newY = y + idy[i];
            if (newX >= 0 && newX < rows && newY >= 0 && newY < cols && rooms[newX][newY] > distance) {
                rooms[newX][newY] = distance + 1;
                dfs(rooms, rows, cols, newX, newY);
            }
        }
    }
}
```


---
###Reference
1. https://leetcode.com/discuss/68456/java-dfs-solution-much-quicker-and-simpler-than-bfs