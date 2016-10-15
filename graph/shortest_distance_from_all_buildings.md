# Shortest Distance from All Buildings

[Leetcode](https://leetcode.com/problems/shortest-distance-from-all-buildings/)


題意：

You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values 0, 1 or 2, where:

- Each 0 marks an empty land which you can pass by freely.
- Each 1 marks a building which you cannot pass through.
- Each 2 marks an obstacle which you cannot pass through.

For example, given three buildings at ```(0,0), (0,4), (2,2)```, and an obstacle at ```(0,2)```:
```
1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```
The point ```(1,2)``` is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal. So return 7.

**Note:**
There will be at least one building. If it is not possible to build such house according to the above rules, return -1.


解題思路：

**難**，很多細節要注意，記住map的xy座標跟我們一般二維陣列的座標不同，使用bfs來作。

>dist[i][j] is the empty land (i, j) to all the buildings.

>grid[i][j] is reused as the accessibility.

```java
public class Solution {
    

    public class Tuple {
        private int y;
        private int x;
        private int dist;
        
        public Tuple(int y, int x, int dist) {
            this.y = y;
            this.x = x;
            this.dist = dist;
        }
    }
    
    
    public int shortestDistance(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }   
        
        int m = grid.length; 
        int n = grid[0].length;
        int[][] dist = new int[m][n];
        
        List<Tuple> buildings = new ArrayList<>();
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    buildings.add(new Tuple(i, j, 0));
                }
                
                grid[i][j] = - grid[i][j];
            }
        }
        
        
        for (int k = 0; k < buildings.size(); k++) {
            bfs(buildings.get(k), k, dist, grid, m, n);
        }
        
        int ans = -1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == buildings.size() && (ans < 0 || dist[i][j] < ans)) {
                    ans = dist[i][j];
                }
            }
        }
        
        return ans;
    }
    
    
    
    int[] idx = {0, 0, -1, 1};
    int[] idy = {-1, 1, 0, 0};
    
    public void bfs(Tuple root, int k, int[][] dist, int[][] grid, int m, int n) {
        Queue<Tuple> q = new LinkedList<>();
        q.offer(root);
        
        while (!q.isEmpty()) {
            Tuple cur = q.poll();
            dist[cur.y][cur.x] += cur.dist;
            
            for (int i = 0; i < 4; i++) {
                int x = cur.x + idx[i];
                int y = cur.y + idy[i];
                if (y >= 0 && x >= 0 && y < m && x < n && grid[y][x] == k) {
                    grid[y][x] = k + 1;
                    q.offer(new Tuple(y, x, cur.dist + 1));
                }
            }
        }
    }
}
```

---
###Reference
1. http://algobox.org/2015/12/26/139/