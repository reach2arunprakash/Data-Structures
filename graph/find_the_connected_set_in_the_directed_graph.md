# Find the Weak Connected Component in the Directed Graph

[原題網址](http://www.lintcode.com/en/problem/find-the-weak-connected-component-in-the-directed-graph/)

解題思路：使用併查集( Union & Find) 來幫助我們找出所有 Connected Components，不斷的遍歷所圖中的所有節點，並不斷的作 union 的動作把相同 component 的節點放入同一個集合中。

```java
public class Solution {
    /**
     * @param nodes a array of Directed graph node
     * @return a connected set of a directed graph
     */
     
    class UnionFind {
        
        HashMap<Integer, Integer> father = new HashMap<Integer, Integer>();
        
        // 先以hashset來初始化father hashmap，每個節點把父親指向自己
        UnionFind(HashSet<Integer> hashSet) {
            for (Integer now : hashSet) {
                father.put(now,now);
            }
        }
        
        // 找到最上層的父節點
        int find(int x) {
            int parent = father.get(x);
            
            while (parent != father.get(parent)) {
                parent = father.get(parent);
            }
            
            return parent;
        }
        
        // 邊找邊壓縮路徑
        int compressed_find(int x) {
            int parent = father.get(x);
            
            //先找到最上層父節點
            while (parent != father.get(parent)) {
                parent = father.get(parent);
            }
            
            int temp = -1;
            int fa = father.get(x);
            
            // 重點，把路徑上的點都指向最上層的父節點
            while (fa != father.get(fa)) {
                temp = father.get(fa);
                father.put(fa,parent); 
                fa = temp;
            }
            
            return parent;
        }
        
        void union(int x, int y) {
            int fa_x = find(x);
            int fa_y = find(y);
            
            if (fa_x != fa_y) {
                father.put(fa_x, fa_y);
            }
        }
    }
    
    List<List<Integer>> print(HashSet<Integer> hashSet, UnionFind uf, int n) {
        
        //key設為parent，value則為同一component的所有節點集合。    
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        HashMap<Integer, List<Integer>> hashMap = new HashMap<Integer, List<Integer>>();
        
        //把相同parent的節點加入到同一個arraylist
        for (int i : hashSet) {
            int fa = uf.find(i);
            
            //找不到則建新的
            if (!hashMap.containsKey(fa)) {
                hashMap.put(fa, new ArrayList<Integer>());
            }
            
            List<Integer> now = hashMap.get(fa);
            now.add(i);
            hashMap.put(fa, now);
        }
        
        //接著再作最後整理，因要按照大小排序好，再發給caller
        for (List<Integer> now: hashMap.values()) {
            Collections.sort(now);
            res.add(now);
        }
        
        return res;
    }

    
    public List<List<Integer>> connectedSet2(ArrayList<DirectedGraphNode> nodes) {
        
        HashSet<Integer> hashSet = new HashSet<Integer>();
        
        //先加入hash set確保沒有重複的節點
        for (DirectedGraphNode now : nodes) {
            hashSet.add(now.label);
            
            for (DirectedGraphNode neighbor : now.neighbors) {
                hashSet.add(neighbor.label);
            }
        }
        
        //接著用hashset建立一個unionfind的物件，
        UnionFind uf = new UnionFind(hashSet);
        
        // 再traverse一次圖，因是有向圖，所以雖然邊找會邊壓縮路徑，
        // 但是最後仍是要確認一下兩者的parent是否相同，若不同，則再union一次
        for (DirectedGraphNode now : nodes) {
            for (DirectedGraphNode neighbor : now.neighbors) {
                int fnow = uf.find(now.label);
                int fneighbor = uf.find(neighbor.label);
                
                if (fnow != fneighbor) {
                    uf.union(now.label, neighbor.label);
                }
            }
        }
        
        return print(hashSet, uf, nodes.size());
    }
}
```

```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) { label = x; neighbors = new ArrayList<DirectedGraphNode>(); }
 * };
 */
public class Solution {
    /**
     * @param nodes a array of Directed graph node
     * @return a connected set of a directed graph
     */
    Map<DirectedGraphNode, DirectedGraphNode> father = new HashMap<DirectedGraphNode, DirectedGraphNode>();
    
    public List<List<Integer>> connectedSet2(ArrayList<DirectedGraphNode> nodes) {
        // union join
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        for ( DirectedGraphNode node : nodes ) {
            father.put(node, node);
        }
        for ( DirectedGraphNode node : nodes ) {
            for ( DirectedGraphNode neighbor : node.neighbors ) {
                if ( find(node) != find(neighbor) ) {
                    union(node, neighbor);
                }
            }
        }
        
        Map<Integer, List<Integer>> map = new HashMap<Integer, List<Integer>>(); 
        
        for ( DirectedGraphNode node : nodes ) {
            if ( !map.containsKey(find(node).label) ) {
                map.put(find(node).label, new ArrayList<Integer>());
            }
            List<Integer> row = map.get(find(node).label);
            row.add(node.label);
        }
        
        for ( List<Integer> row : map.values() ) {
            Collections.sort(row);
            result.add(row);
        }
        return result;
        
    }
    
    public void union(DirectedGraphNode node1, DirectedGraphNode node2) {
        DirectedGraphNode fa_1 = find(node1);
        DirectedGraphNode fa_2 = find(node2);
        
        if ( fa_1 != fa_2 ) {
            father.put(fa_1, fa_2);
        }
    }
    
    public DirectedGraphNode find(DirectedGraphNode node) {
        DirectedGraphNode parent = father.get(node);
        while ( parent != father.get(parent) ) {
            parent = father.get(parent);
        }
        return parent;
    }
}

```

