# Find the Connected Component in the Undirected Graph

[原題網址](http://www.lintcode.com/en/problem/find-the-connected-component-in-the-undirected-graph/)

> Find the number connected component in the undirected graph. Each node in the graph contains a label and a list of its neighbors. (a connected component (or just component) of an undirected graph is a subgraph in which any two vertices are connected to each other by paths, and which is connected to no additional vertices in the supergraph.)

解題思路：對於圖中的每個點，進行深度搜尋，並用一個visited map來判所該點是否之前被訪問過，若是，則忽略，若無則拜訪，因此為無向圖，所以同一個component的點都會在一次深度搜尋後被訪問到。

```java
public List<List<Integer>> connectedSet(ArrayList<UndirectedGraphNode> nodes) {
    
    Map<UndirectedGraphNode, Integer> visited = new HashMap<UndirectedGraphNode, Integer>();
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    for ( UndirectedGraphNode node : nodes ) {
        if ( visited.containsKey(node) ) {
            continue;
        }
        Queue<UndirectedGraphNode> queue = new LinkedList<UndirectedGraphNode>();
        queue.offer(node);
        List<Integer> set = new ArrayList<Integer>();
        
        while ( !queue.isEmpty() ) {
            UndirectedGraphNode current = queue.poll();
            if ( visited.containsKey(current) ) {
                continue;
            }
            set.add(current.label);
            visited.put(current, 1);
            for ( UndirectedGraphNode neighbor : current.neighbors ) {
                if ( visited.containsKey(neighbor) ) {
                    continue;
                } else {
                    queue.offer(neighbor);   
                }
            }
        }
        Collections.sort(set);
        result.add(set);
    }
    return result;
}
```
