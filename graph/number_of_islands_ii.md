#Number of Island II

[原題連結](http://www.lintcode.com/en/problem/number-of-islands-ii/)

題意：為第一題的變形，地圖會根據不同時間變化，因此無法直接使用深度搜尋來解決。

解題思路：可以使用暴力法，每次地圖變更時，便掃一次地圖，但是需要花 $$O(K * N^{2})$$的時間複雜度，N為地圖的長度與寬度，K 為地圖變化的次數。

另一個我們可以使用的就是Union & Find 來解決這道題，每次新增一個小島，便將島嶼個數加一，並且去搜尋該島嶼的上下左右是否有鄰近島嶼，若有的話，則作 Union且將島嶼個數減一。即每次出現一個小島，都可以把它當作一個集合合併的過程。

需要注意的地方就是我們將坐標轉為 index 會幫助我們較方便的處理，因此另寫了一個坐標轉換的程式。此題還有一個可以優化的地方，即find 可改為compress find，以利縮短路徑長度。


```java
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */
public class Solution {
    /**
     * @param n an integer
     * @param m an integer
     * @param operators an array of point
     * @return an integer array
     */
     
    public class UnionFind {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        
        public UnionFind(int n, int m) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    int idx = convertIdx(i, j, m);
                    map.put(idx, idx);
                }
            }
        }
        
        public int find(int x) {
            int parent = map.get(x);
            while (parent != map.get(parent)) {
                parent = map.get(parent);
            }
            
            return parent;
        }
        
        public void union(int x, int y) {
            int parentX = find(x);
            int parentY = find(y);
            
            if (parentX != parentY) {
                map.put(parentX, parentY);
            }
        }
    }
    
    public int convertIdx(int i, int j, int m) {
            return i * m + j;
    }
    
    public List<Integer> numIslands2(int n, int m, Point[] operators) {
        
        List<Integer> res = new ArrayList<Integer>();
        
        if (operators == null || operators.length == 0) {
            return res;
        }
        
        // 設定上下左右的標準offset
        int[][] directions = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        boolean[][] isIsland = new boolean[n][m];
        
        UnionFind uf = new UnionFind(n, m);
        int count = 0;
        
        // 每一個新的島嶼出來，便將個數加一，再去檢查上下左右來決定是否聯集。
        for (int i = 0; i < operators.length; i++) {
            int x = operators[i].x;
            int y = operators[i].y;
            int index = convertIdx(x, y, m);
            
            isIsland[x][y] = true;
            count++;
            
            // 檢查該島嶼的上下左右，若有島嶼，且老大不同，則進行聯集，並將個數減一
            for (int j = 0; j < directions.length; j++) {
                int newX = x + directions[j][0];
                int newY = y + directions[j][1];
                int newIndex = convertIdx(newX, newY, m);
                
                if (newX >= 0 && newX < n && newY >= 0 && newY < m) {
                    
                    if (isIsland[newX][newY]) {
                        
                        int father = uf.find(index);
                        int neighborFather = uf.find(newIndex);
                        if (father != neighborFather) {
                            count--;
                            uf.union(index, newIndex);
                        }
                    }
                }
            }
            res.add(count);
        }
        
        return res;
    }
}

```

updated 2015.11.15

其實不需要實作整套的union & find，我們只需要quick find即可，其程式碼如下：

> 記住 index的轉換為 x * n + y，而非x * m + y

```java
public class Solution {
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        
        // 直接使用一維陣列即可
        int[] roots = new int[m * n];
        Arrays.fill(roots, -1);
        
        int[] xOffset = {0, 0, 1, -1};
        int[] yOffset = {1, -1, 0, 0};
        int count = 0;
        List<Integer> res = new ArrayList<Integer>();
        for (int i = 0; i < positions.length; i++) {
            int x = positions[i][0];
            int y = positions[i][1];
            roots[x * n + y] = x * n + y;
            
            count++;
            
            for (int j = 0; j < 4; j++) {
                int newX = x + xOffset[j];
                int newY = y + yOffset[j];
                if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                    if (roots[newX * n + newY] != -1) {
                        int root1 = find(x * n + y, roots);
                        int root2 = find(newX * n + newY, roots);
                        if (root1 != root2) {
                            count--;
                            roots[root1] = root2;
                        }
                    }
                }
            }
            res.add(count);
        }
        
        return res;
    }
    
    private int find(int target, int[] roots) {
        if (roots[target] == target) {
            return target;
        }
        roots[target] = find(roots[target], roots);
        return roots[target];
    }
}
```

>時間複雜度：$$O(M \times N \times K)$$