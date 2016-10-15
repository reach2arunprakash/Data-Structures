# Paint House

[Leetcode](https://leetcode.com/problems/paint-house/)


題意：

There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a n x 3 cost matrix. For example, costs[0][0] is the cost of painting house 0 with color red; costs[1][2] is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

Note:
All costs are positive integers.


解題思路：


>dp[i][0] = costs[i][0] + min(costs[i - 1][1], costs[i - 1][2])

>表示第i個房子上紅色的最小成本，因為第i個房子上了紅色，所以他之前的房子只可能上綠色與藍色。

```java
public int minCost(int[][] costs) {
        if(costs != null && costs.length == 0) return 0;
        for(int i = 1; i < costs.length; i++){
            // 涂第一種顏色的話，上一個房子就不能涂第一種顏色，這樣我們要在上一個房子的第二和第三個顏色的最小開銷中找最小的那個加上
            costs[i][0] = costs[i][0] + Math.min(costs[i - 1][1], costs[i - 1][2]);
            // 涂第二或者第三種顏色同理
            costs[i][1] = costs[i][1] + Math.min(costs[i - 1][0], costs[i - 1][2]);
            costs[i][2] = costs[i][2] + Math.min(costs[i - 1][0], costs[i - 1][1]);
        }
        // 返回涂三種顏色中開銷最小的那個
        return Math.min(costs[costs.length - 1][0], Math.min(costs[costs.length - 1][1], costs[costs.length - 1][2]));
    }
```

---
###Refernce
1. http://massivealgorithms.blogspot.com/2015/06/leetcode-256-painting-house-neverlandly.html