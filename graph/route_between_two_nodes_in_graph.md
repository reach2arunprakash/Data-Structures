# Route Between Two Nodes in Graph

[Lintcode](http://www.lintcode.com/en/problem/route-between-two-nodes-in-graph/)

題意：

Given a directed graph, design an algorithm to find out whether there is a route between two nodes.


Example

Given graph:
```
A----->B----->C
 \     |
  \    |
   \   |
    \  v
     ->D----->E

for s = B and t = E, return true

for s = D and t = C, return false
```

解題思路：

使用 DFS或BFS皆可，在此使用DFS


遞迴解法：

```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) {
 *         label = x;
 *         neighbors = new ArrayList<DirectedGraphNode>();
 *     }
 * };
 */
public class Solution {
   /**
     * @param graph: A list of Directed graph node
     * @param s: the starting Directed graph node
     * @param t: the terminal Directed graph node
     * @return: a boolean value
     */
    public boolean hasRoute(ArrayList<DirectedGraphNode> graph, 
                            DirectedGraphNode s, DirectedGraphNode t) {
        
        if (graph == null || graph.size() == 0 || s == null || t == null) {
            return false;
        }
        
        return dfs(s, t, new HashSet<DirectedGraphNode>());
    }
    
    public boolean dfs(DirectedGraphNode s, DirectedGraphNode t, Set<DirectedGraphNode> visited) {
        if (s == null) {
            return false;
        }
        if (s.equals(t)) {
            return true;
        }
        visited.add(s);
        for (DirectedGraphNode neighbor : s.neighbors) {
            if (visited.contains(neighbor)) {
                continue;
            }
            if (dfs(neighbor, t, visited)) {
                return true;
            }
        }
        
        return false;
    }
}

```

非遞迴解法：

```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) {
 *         label = x;
 *         neighbors = new ArrayList<DirectedGraphNode>();
 *     }
 * };
 */
public class Solution {
   /**
     * @param graph: A list of Directed graph node
     * @param s: the starting Directed graph node
     * @param t: the terminal Directed graph node
     * @return: a boolean value
     */
    public boolean hasRoute(ArrayList<DirectedGraphNode> graph, 
                            DirectedGraphNode s, DirectedGraphNode t) {
        
        
        Set<DirectedGraphNode> visited = new HashSet<DirectedGraphNode>();
        Stack<DirectedGraphNode> stack = new Stack<DirectedGraphNode>();
        stack.push(s);
        while (!stack.isEmpty()) {
            DirectedGraphNode cur = stack.pop();
            
            if (cur.equals(t)) {
                return true;
            }
            
            for (DirectedGraphNode neighbor : cur.neighbors) {
                stack.push(neighbor);
            }
        }
        
        return false;
    }
}

```

---
###Reference
1. http://cherylintcode.blogspot.com/2015/07/route-between-two-nodes-in-graph.html