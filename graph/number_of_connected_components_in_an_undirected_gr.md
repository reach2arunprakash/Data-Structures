# Number of Connected Components in an Undirected Graph

[Leetcode](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

題意：


解題思路：


可以使用dfs，bfs，或是union & find來解, 在此我們先用union & find來解。

1. 先初始化每個節點的父親是自己
2. 不斷的把相連的兩個點作union
3. 把father的數量算出來即可


```java
public class Solution {
    
    int[] father;
    public int countComponents(int n, int[][] edges) {
        father = new int[n];
        
        for (int i = 0; i < n; i++) {
            father[i] = i;
        }
        
        for (int i = 0; i < edges.length; i++) {
            union(edges[i][0], edges[i][1]);
        }
        
        Set<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < n; i++) {
            set.add(find(i));
        }
        
        return set.size();
        
    }
    
    
    private int find(int node) {
        if (father[node] == node) {
            return node;
        }
        
        while (father[node] != node) {
            node = father[father[node]];
        }
        
        return node;
    }
    
    private int union(int nodeOne, int nodeTwo) {
        int fatherOne = find(nodeOne);
        int fatherTwo = find(nodeTwo);
        if (fatherOne != fatherTwo) {
            father[fatherOne] = fatherTwo;
        }
        
        return fatherTwo;
    }
}
```

---
###Reference
1. https://leetcode.com/discuss/76508/ac-java-code-union-find