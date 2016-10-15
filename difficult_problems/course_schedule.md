# Course Schedule

[Leetcode](https://leetcode.com/problems/course-schedule/)

題意：

There are a total of n courses you have to take, labeled from 0 to n - 1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

For example:

```2, [[1,0]]```

There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

```2, [[1,0],[0,1]]```

There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

Note:
The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.

解題思路：

以下為網友 [Grandyang](http://www.cnblogs.com/grandyang/p/4484571.html) 的解說：

"下面我們來看DFS的解法，也需要建立有向圖，還是用二維數組來建立，和BFS不同的是，我們像現在需要一個一維數組visit來記錄訪問狀態，大體思路是，先建立好有向圖，然後從第一個門課開始，找其可構成哪門課，暫時將當前課程標記為已訪問，然後對新得到的課程調用DFS遞歸，直到出現新的課程已經訪問過了，則返回false，沒有衝突的話返回true，然後把標記為已訪問的課程改為未訪問。代碼如下："

其程式碼如下：

```java
public class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        ArrayList[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new ArrayList();
        }
        
        boolean[] visited = new boolean[numCourses];
        for (int i = 0; i < prerequisites.length; i ++) {
            graph[prerequisites[i][1]].add(prerequisites[i][0]);
        }
        
        for (int i = 0; i < numCourses; i++) {
            if (!dfs(graph, visited, i)) {
                return false;
            }
        }
        return true;
    }
    
    private boolean dfs (ArrayList[] graph, boolean[] visited, int course) {
        if (visited[course]) {
            return false;
        } else {
            visited[course] = true;
        }
        
        for (int i = 0; i < graph[course].size(); i++) {
            if (!dfs(graph, visited, (int)graph[course].get(i))) {
                return false;
            }
        }
        visited[course] = false;
        return true;
    }
}
```


updated 2015.11.27

用Topological sort來作，課程當節點，先修課與課相連當邊，不斷造訪indegree為0的點，其程式碼如下：

```java
public class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> posts = new ArrayList<List<Integer>>();
        for (int i = 0; i < numCourses; i++) {
            posts.add(new ArrayList<Integer>());
        }
        
        int[] preNums = new int[numCourses];
        for (int i = 0; i < prerequisites.length; i++) {
            posts.get(prerequisites[i][1]).add(prerequisites[i][0]);
            preNums[prerequisites[i][0]]++;
        }
        
        Queue<Integer> queue = new LinkedList<Integer>();
        for (int i = 0; i < numCourses; i++) {
            if (preNums[i] == 0){
                queue.offer(i);
            }
        }
        
        int count = numCourses;
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (int i : posts.get(cur)) {
                if (--preNums[i] == 0) {
                    queue.offer(i);
                }
            }
            count--;
        }
        
        return count == 0;
    }
}
```

Course Schedule II 

與1完全一樣，只需要加個ordering的陣列來依序把課程編號加入。

```java
public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] ordering = new int[numCourses];
        Arrays.fill(ordering, -1);
        List<List<Integer>> lists = new ArrayList<List<Integer>>();
        for (int i = 0; i < numCourses; i++) {
            lists.add(new ArrayList<Integer>());
        }
        
        int[] preNum = new int[numCourses];
        for (int i = 0; i < prerequisites.length; i++) {
            lists.get(prerequisites[i][1]).add(prerequisites[i][0]);
            preNum[prerequisites[i][0]]++;
        }
        
        Queue<Integer> q = new LinkedList<Integer>();
        
        for (int i = 0; i < numCourses; i++) {
            if (preNum[i] == 0) {
                q.offer(i);
            }
        }
        
        int idx = 0;
        int count = numCourses;
        while(!q.isEmpty()) {
            int cur = q.poll();
            ordering[idx++] = cur;
            for (int i : lists.get(cur)) {
                if (--preNum[i] == 0) {
                    q.offer(i);
                }
            }
            count--;
        }
        
        if (count == 0) {
            return ordering;
        } else {
            return new int[0];
        }
    }
}
```

---
###Reference
1. http://www.cnblogs.com/grandyang/p/4484571.html