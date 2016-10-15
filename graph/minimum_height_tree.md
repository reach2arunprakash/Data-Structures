# Minimum Height Tree

[Leetcode](https://leetcode.com/problems/minimum-height-trees/)

題意：

For a undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees (MHTs). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

Format
The graph contains n nodes which are labeled from 0 to n - 1. You will be given the number n and a list of undirected edges (each edge is a pair of labels).

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

Example 1:

Given n = 4, edges = [[1, 0], [1, 2], [1, 3]]
```
        0
        |
        1
       / \
      2   3
      ```
return [1]

Example 2:

Given n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]
```
     0  1  2
      \ | /
        3
        |
        4
        |
        5
```
return [3, 4]


解題思路：

給定節點樹目與相連(edge)關係，我們要找出最小高度的樹之根節點，為了要讓樹高度行越小，我們盡量要讓degree高的當作root。

我們可以利用topological sort來不斷刪除indegree為1的節點，最後剩下的一層節點即是我們所要的root集合，其程式碼如下：


```java
public class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer> res = new ArrayList<>();
        
        
        if (n == 1) {
            res.add(0);
            return res;
        }
        
        if (n == 0 || edges == null || edges.length == 0) {
            return res;
        }
        
        List<Set<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new HashSet<Integer>());
        }
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        
        List<Integer> leaves = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (adj.get(i).size() == 1) {
                leaves.add(i);
            }
        }
        
        while (n > 2) {
            n -= leaves.size();
            List<Integer> newLeaves = new ArrayList<>();
            for (int i : leaves) {
                int j = adj.get(i).iterator().next();
                adj.get(j).remove(i);
                if (adj.get(j).size() == 1) {
                    newLeaves.add(j);
                }
            }
            
            leaves = newLeaves;
        }
        
        return leaves;
        
    }
}
```