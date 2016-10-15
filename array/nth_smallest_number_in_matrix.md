#Kth Smallest Number in Sorted Matrix

[原題網址](http://www.lintcode.com/en/problem/kth-smallest-number-in-sorted-matrix/)

題意：給定一個排序好的二維矩陣，找出矩陣中第 k 大的值。

解題思路：可使用 priority queue 來輔助我們完成這道題，由矩陣的左上元素開始向兩旁延伸，有點類似 dfs 的概念，周圍未走過的元素加到 queue 中，利用 priority queue 的特性，每次pop出 queue 中最小的元素，每 pop 一次，則 k - 1，直到 k = 0 時，queue 的頂端元素即是我們要找的值，其原始碼如下：

> 在此我們實作了 Cell 的 class 來存放矩陣元素的座標與值。

```java
public class Solution {
    /**
     * @param matrix: a matrix of integers
     * @param k: an integer
     * @return: the kth smallest number in the matrix
     */
    // 實作 Cell 來存放座標與值 
    class Cell implements Comparator<Cell>{
        int x;
        int y;
        int val;
        
        public Cell() {}
        
        public Cell(int x, int y, int val) {
            this.x = x;
            this.y = y;
            this.val = val;
        }
        
        public int compare(Cell a, Cell b) {
            return a.val - b.val;
        }
    }
    
    public int kthSmallest(int[][] matrix, int k) {
        
        PriorityQueue<Cell> q = new PriorityQueue<Cell>(1, new Cell());
        
        if (matrix == null || matrix.length == 0) {
            return 0;
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        int res = 0;
        boolean[][] visited = new boolean[rows][cols];
        
        // 由 matrix 的左上開始走，不斷地向旁邊延伸，因旁邊的代表是與當前值差距最小的元素。
        // 有點類似dfs 的走法。
        q.offer(new Cell(0, 0, matrix[0][0]));
        visited[0][0] = true;
        while(k > 1) {
            
            // 每彈出一次則k - 1，等k = 0時，q中的top即是我們要找的數。
            Cell c = q.poll();
            k--;
            
            // 向右邊探索，若還有值且還沒走過，則加入 queue
            if (c.x + 1 < rows && visited[c.x + 1][c.y] == false) {
                q.offer(new Cell(c.x + 1, c.y, matrix[c.x + 1][c.y]));
                visited[c.x + 1][c.y] = true;
            }
            
            
            // 向下面探索，若還有值且還沒走過，則加入 queue
            if (c.y + 1 < cols && visited[c.x][c.y + 1] == false) {
                q.offer(new Cell(c.x, c.y + 1, matrix[c.x][c.y + 1]));
                visited[c.x][c.y + 1] = true;
            }
        }
        
        return q.poll().val;
        
    }
}

>Time Complexity：O(KlogN), n is the maximal number in width and height.

---
###Reference
1. http://cherylintcode.blogspot.com/2015/07/kth-smallest-number-in-sorted-matrix.html