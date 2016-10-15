# The Skyline Problem

[Leetcode](https://leetcode.com/problems/the-skyline-problem/)

題意：畫出建築物的輪櫎

解題思路：

把每個building的上升邊與下降邊分別放入 list 並作排序，排放順序如下

1. pos 較小的放前
2. 若pos 一樣，如果同為上升邊，把較高的放前面
3. 若是下降邊，把較矮的放前面
4. 只有其中一邊是上升，把上升邊放前面

接著再不斷的根據邊的不同對 haep作操作

1. 若為上升邊，則比較頂端高度輸出一個edge，再把自己加入
2. 若為下降邊，則比較頂端高度

接著

```java
public class Solution {
    
    class Edge implements Comparator<Edge> {
        int pos;
        int height;
        boolean isStart;
        public Edge(int pos, int height, boolean isStart) {
            this.pos = pos;
            this.height = height;
            this.isStart = isStart;
        }
        
        public Edge(){}
        
        public int compare(Edge edgeOne, Edge edgeTwo) {
            if (edgeOne.pos != edgeTwo.pos) {
                return Integer.compare(edgeOne.pos, edgeTwo.pos);
            } else if (edgeOne.isStart && edgeTwo.isStart) {
                return Integer.compare(edgeTwo.height, edgeOne.height); 
            } else if (!edgeOne.isStart && !edgeTwo.isStart) {
                return Integer.compare(edgeOne.height, edgeTwo.height); // 如果兩個都是下降，則較短的在前，較高的在後
            } else {
                return edgeOne.isStart ? -1 : 1;
            }
        }
    }
    
    public List<int[]> getSkyline(int[][] buildings) {
        
        List<int[]> res = new ArrayList<int[]>();
        
        if (buildings == null || buildings.length == 0 ||buildings[0].length == 0) {
            return res;
        }
        
        List<Edge> edges = new ArrayList<Edge>();
        for (int[] building : buildings) {
            Edge startEdge = new Edge(building[0], building[2], true);
            edges.add(startEdge);
            Edge endEdge = new Edge(building[1], building[2], false);
            edges.add(endEdge);
        }
        
        Collections.sort(edges, new Edge());
        
        // min heap 把時間小放在前面
        PriorityQueue<Integer> heap = new PriorityQueue<Integer>(1, Collections.reverseOrder());
        for (Edge edge : edges) {
            if (edge.isStart) {
                if (heap.isEmpty() || edge.height > heap.peek()) {
                    res.add(new int[]{edge.pos, edge.height});
                }
                heap.add(edge.height);
            } else {
                heap.remove(edge.height);
                if (heap.isEmpty() || edge.height > heap.peek()) {
                    res.add(heap.isEmpty() ? new int[]{edge.pos, 0} : new int[]{edge.pos, heap.peek()}); 
                }
            }
        }
        
        return res;
    }
}
```