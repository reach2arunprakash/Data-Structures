# Graph Valid Tree

[Leetcode](https://leetcode.com/problems/graph-valid-tree/)

題意：


解題思路：

法一：使用dfs來判斷有沒有cycle，若沒有cycle，則判斷是否所有點皆走過，有2個case沒過，之後再來修正，其程式碼如下，有點長。

>由於是無向圖，在往下走dfs時，記得判斷當前neighbor是否與之前parent相同。

```java
public class Solution {
    HashMap<Integer, List<Integer>> neighbors = new HashMap<Integer, List<Integer>>();
    HashSet<Integer> onPath = new HashSet<Integer>();
    boolean[] visited;
    public boolean validTree(int n, int[][] edges) {
        if (n == 0 || edges.length == 0) {
            return true;
        }
        
        
        for (int i = 0; i < edges.length; i++) {
            if (!neighbors.containsKey(edges[i][1])) {
                neighbors.put(edges[i][1], new ArrayList<Integer>());
            }
            neighbors.get(edges[i][1]).add(edges[i][0]);
            if (!neighbors.containsKey(edges[i][0])) {
                neighbors.put(edges[i][0], new ArrayList<Integer>());
            }
            neighbors.get(edges[i][0]).add(edges[i][1]);
        }
        
        visited = new boolean[n];
        Arrays.fill(visited, false);
        
        if (hasCycle(0, -1)) {
            return false;
        }
        
        for (int i = 0; i < visited.length; i++) {
            if (visited[i] == false) {
                return false;
            }
        }
        
        return true;
    }
    
    private boolean hasCycle(int vertax, int parent) {
        if (visited[vertax] == true) {
            return true;
        }
        visited[vertax] = true;
        onPath.add(vertax);
        
        if (neighbors.containsKey(vertax)) {
            for (Integer neighbor : neighbors.get(vertax)) {
                // 因是無向圖，所以要注意來源與目的地
                if (neighbor != parent && hasCycle(neighbor, vertax)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

法二：使用union & find，走過每個edge，如果中除找到兩個點的father相同，代表之前有走過而形成cycle，返回false，且改良過的union find需要多一個count來紀錄component的數目，最後要判斷count是否只為1，代表圖是否連通來決定是否是一棵樹，其程式碼如下：

```java
public class Solution {
    
    public class UnionFind {
        HashMap<Integer, Integer> father;
        int count;
        public UnionFind(int n) {
            father = new HashMap<Integer, Integer>();
            for (int i = 0; i < n; i++) {
                father.put(i, i);
            }
            count = n;
        }
        
        public int find(int n) {
            int parent = father.get(n);
            while(parent != father.get(parent)) {
                parent = father.get(parent);
            }
            return parent;
        }
        
        public void union(int a, int b) {
            int fatherA = find(a);
            int fatherB = find(b);
            if (fatherA != fatherB) {
                father.put(fatherB, fatherA);
            }
            count--;
        }
    }

    public boolean validTree(int n, int[][] edges) {
        if (n == 0) {
            return true;
        }
        UnionFind uf = new UnionFind(n); 
        for (int i = 0; i < edges.length; i++) {
            int vertax = edges[i][1];
            int neighbor = edges[i][0];
            if (uf.find(vertax) == uf.find(neighbor)) {
                return false;
            }
            uf.union(vertax, neighbor);
        }
        
        return uf.count == 1 ? true : false;
        
    }
}
```

法三：使用quick-union來作

```java
public class Solution {
    public boolean validTree(int n, int[][] edges) {
        int[] root = new int[n]; // 類似union find的father hashmap
        for (int i = 0; i < n; i++) {
            root[i] = i; // 作初始化，每個節點為自己的父親
        }
        
        for (int i = 0; i < edges.length; i++) {
            int root1 = find(root, edges[i][0]);
            int root2 = find(root, edges[i][1]);
            if (root1 == root2) {
                return false;
            }
            root[root2] = root1;
        }
        return edges.length == n - 1;
    }
    
    private int find(int[] root, int e) {
        if (root[e] == e) {
            return e;
        } else {
            return find(root, root[e]);
        }
    }
}
```

---
###Reference
1. http://www.cnblogs.com/jcliBlogger/p/4738788.html
2. http://likesky3.iteye.com/blog/2240154
3. http://www.fgdsb.com/2015/02/16/valid-tree/