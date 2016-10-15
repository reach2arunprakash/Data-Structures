#Building Outline

[原題網址](http://www.lintcode.com/en/problem/building-outline/)


解題思路：

###難~

根據 [網友](https://codesolutiony.wordpress.com/2015/06/01/leetcode-the-skyline-problem-lintcode-building-outline/) 提供以下思路：

把每一個building拆成兩個edge，一個入一個出。所有的edge加入到一個list中。再對這個list進行排序。

**排序順序為：如果兩個邊的position不一樣，那麼按pos排，否則根據edge是入還是出來排。**

根據position從前到後掃瞄每一個edge，將edge根據是入還是出來將當前height加入或者移除heap。再得到當前最高點來決定是否加入最終結果。非常巧妙，值得思考。

```java
public class Solution {
    /**
     * @param buildings: A list of lists of integers
     * @return: Find the outline of those buildings
     */
    class Edge implements Comparator<Edge> {
        int pos;
        int height;
        boolean isStart;
        public Edge(int pos, int height, boolean isStart) {
            this.pos= pos;
            this.height = height;
            this.isStart = isStart;
        }
        // 
        public int compare(Edge e1, Edge e2) {
            if (e1.pos != e2.pos) {
                return Integer.compare(e1.pos, e2.pos);
            }
            
            if (e1.isStart && e2.isStart) {
                return Integer.compare(e1.height, e2.height);
            }
            
            if (!e1.isStart && !e2.isStart) {
                return Integer.compare(e1.height, e2.height);
            }
            
            // 表示只有其中一邊是start
            return e1.isStart ? -1 : 1;
        }
        
        public Edge() {}
    }
    
    private ArrayList<ArrayList<Integer>> output(ArrayList<ArrayList<Integer>> res) {
        
        ArrayList<ArrayList<Integer>> ans = new ArrayList<ArrayList<Integer>>();
        if (res.size() > 0) {
            int pre = res.get(0).get(0);
            int height = res.get(0).get(1);
            for (int i = 1; i < res.size(); i++) {
                ArrayList<Integer> now = new ArrayList<Integer>();
                int id = res.get(i).get(0);
                if (height > 0) {
                    now.add(pre);
                    now.add(id);
                    now.add(height);
                    ans.add(now);
                }
                pre = id;
                height = res.get(i).get(1);
            }
        }
        
        return ans;
    }
    public ArrayList<ArrayList<Integer>> buildingOutline(int[][] buildings) {
        
        ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
        
        if (buildings == null || buildings.length == 0) {
            return res;
        }
        
        
        // 先把所有邊全加入 list 中，並標記是start或是end
        List<Edge> edges = new ArrayList<Edge>();
        for (int[] building : buildings) {
            Edge startEdge = new Edge(building[0], building[2], true);
            edges.add(startEdge);
            Edge endEdge = new Edge(building[1], building[2], false);
            edges.add(endEdge);
        }
        
        // sort edges according to position, height, and if the edge is start or end
        Collections.sort(edges, new Edge());
        
        PriorityQueue<Integer> heap = new PriorityQueue<Integer>();
        ArrayList<Integer> now = null;
        for (Edge edge : edges) {
            if (edge.isStart) {
                if (heap.isEmpty() || edge.height > heap.peek()) {
                    now = new ArrayList<Integer>(Arrays.asList(edge.pos, edge.height));
                    res.add(now);
                }
                heap.add(edge.height);
            } else {
                heap.remove(edge.height);
                if (heap.isEmpty() || edge.height > heap.peek()) {
                    if (heap.isEmpty()) {
                        now = new ArrayList<Integer>(Arrays.asList(edge.pos, 0));
                    } else {
                        now = new ArrayList<Integer>(Arrays.asList(edge.pos, heap.peek()));
                    }
                    
                    res.add(now);
                }
            }
        }
        
        return output(res);
    }
}

```

---
###Reference
1. https://codesolutiony.wordpress.com/2015/06/01/leetcode-the-skyline-problem-lintcode-building-outline/