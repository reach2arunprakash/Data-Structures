# Best Meeting Point

[Leetcode](https://leetcode.com/problems/best-meeting-point/)

題意：

A group of two or more people wants to meet and minimize the total travel distance. You are given a 2D grid of values 0 or 1, where each 1 marks the home of someone in the group. The distance is calculated using Manhattan Distance, where distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|.

For example, given three people living at (0,0), (0,4), and (2,2):
```
1 - 0 - 0 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```
The point (0,2) is an ideal meeting point, as the total travel distance of 2+2+2=6 is minimal. So return 6.

Hint:

Try to solve it in one dimension first. How can this solution apply to the two dimension case?

解題思路：

網友提供以下思路：

"為了保證總長度最小，我們只要保證每條路徑儘量不要重複就行了，比如1->2->3<-4這種一維的情況，如果起點是1，2和4，那2->3和1->2->3這兩條路徑就有重複了。為了儘量保證右邊的點向左走，左邊的點向右走，那我們就應該去這些點中間的點作為交點。由於是曼哈頓距離，我們可以分開計算橫坐標和縱坐標，結果是一樣的。所以我們算出各個橫坐標到中點橫坐標的距離，加上各個縱坐標到中點縱坐標的距離，就是結果了。"

其程式碼如下：

```java
public class Solution {
    public int minTotalDistance(int[][] grid) {
        List<Integer> ipos = new ArrayList<Integer>();
        List<Integer> jpos = new ArrayList<Integer>();
        // 統計出有哪些橫縱坐標
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                if(grid[i][j] == 1){
                    ipos.add(i);
                    jpos.add(j);
                }
            }
        }
        int sum = 0;
        // 計算縱坐標到縱坐標中點的距離，這裡不需要排序，因為之前統計時是按照i的順序
        for(Integer pos : ipos){
            sum += Math.abs(pos - ipos.get(ipos.size() / 2));
        }
        // 計算橫坐標到橫坐標中點的距離，這裡需要排序，因為統計不是按照j的順序
        Collections.sort(jpos);
        for(Integer pos : jpos){
            sum += Math.abs(pos - jpos.get(jpos.size() / 2));
        }
        return sum;
    }
}
```

---
###Reference
1. http://segmentfault.com/a/1190000003894693