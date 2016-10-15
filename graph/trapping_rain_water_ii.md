#Trapping Rain Water II

[原題網址](http://www.lintcode.com/en/problem/trapping-rain-water-ii/)
題意：仍然是找出灌水的總容積，為 Trapping Rain Water的follow up，一維改成二維。

解題思路：觀念類似於第一題，但在這裡是將圖中的最外圍全放進優先序列(Priority Queue)中，其中越低的高度越前面，再來便不斷的將優先序列前面的值拿出來，再與上下左右的鄰居作比較。

```java
public class Solution {
    /**
     * @param heights: a matrix of integers
     * @return: an integer
     */
     
    class Pillar {
        int x, y, height;
        Pillar(int x, int y, int height) {
            this.x = x;
            this.y = y;
            this.height = height;
        }
    }
    
    class PillarComparator implements Comparator<Pillar> {

        public int compare(Pillar a, Pillar b) {
            
            if (a.height > b.height) {
                return 1;
            } else if (a.height == b.height) {
                return 0;
            } else {
                return -1;
            }
        }
    }
    
    public int trapRainWater(int[][] heights) {
        
        if (heights == null || heights.length == 0) {
            return 0;
        }
        
        int rows = heights.length;
        int cols = heights[0].length;
        int[][] directions = new int[][] {{0, 1}, {0, -1}, {1, 0}, {-1,0}};
        boolean[][] visited = new boolean[rows][cols];
        PriorityQueue<Pillar> queue = new PriorityQueue<Pillar>(1, new PillarComparator());
        
        // 下面兩個循環主要是把最外圈所有元素皆放進queue中
        // 第一個循環是放第一列跟最後一列
        for (int i = 0; i < cols; i++) {
            queue.offer(new Pillar(0, i, heights[0][i]));
            queue.offer(new Pillar(rows - 1, i, heights[rows - 1][i]));
            visited[0][i] = true;
            visited[rows - 1][i] = true;
        }
        
        // 第二個循環是放第一行與最後一行
        for (int i = 1; i < rows - 1; i ++) {
            queue.offer(new Pillar(i, 0, heights[i][0]));
            queue.offer(new Pillar(i, cols - 1, heights[i][cols - 1]));
            visited[i][0] = true;
            visited[i][cols - 1] = true;
        }
        
        // 接著不斷從 queue 中挑出最小的值出來處理
        int ans = 0;
        
        while (!queue.isEmpty()) {
            
            Pillar cur = queue.poll();
            
            for (int i = 0; i < directions.length; i++) {
                int newX = cur.x + directions[i][0];
                int newY = cur.y + directions[i][1];
                
                // 確保鄰居沒被訪問過且也在陣列範圍內
                if (newX >= 0 && newX < rows && newY >= 0 && newY < cols && !visited[newX][newY]) {
                    
                    visited[newX][newY] = true;
                    // 記得trap I的是找最高的，所以記得跟原本的高度作比較再offer進 queue中。
                    queue.offer(new Pillar(newX, newY, Math.max(cur.height,heights[newX][newY])));
                    
                    // 重點是怎麼處理答案，就是把目前的高度與鄰居的高度相減，如果鄰居較低(把自己當牆)，
                    // 則代表我們可以灌水進去，否則放棄。
                    ans += Math.max(0, cur.height - heights[newX][newY]);
                }
            }
        }
        
        return ans;
        
    }
};
```
