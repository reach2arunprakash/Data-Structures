# Paint House II

[Leetcode](https://leetcode.com/problems/paint-house-ii/)


題意：

There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a n x k cost matrix. For example, costs[0][0] is the cost of painting house 0 with color 0; costs[1][2] is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.

Note:
All costs are positive integers.

Follow up:
Could you solve it in O(nk) runtime?

解題思路：

使用dp來幫忙解，此複雜度O(N*K*K)


>dp[i][j] = min(dp[i - 1][k]) + costs[i][j], 0 <= k < colors && k != j

>dp[i][j] 表示第i橦房子必塗第j顏色的最小成本是什麼。

程式碼如下：

```java
public class Solution {
    public int minCostII(int[][] costs) {
        if (costs == null || costs.length == 0) {
            return 0;
        }
        
        int houses = costs.length;
        int colors = costs[0].length;
        
        
        int[][] dp = new int[houses][colors];
        for (int i = 0; i < colors; i++) {
            dp[0][i] = costs[0][i]; 
        }
        
        for (int i = 1; i < houses; i++) {
            for (int j = 0; j < colors; j++) {
                dp[i][j] = Integer.MAX_VALUE;
                for (int k = 0; k < colors; k++) {
                    if (k != j) {
                        dp[i][j] = Math.min(dp[i - 1][k] + costs[i][j], dp[i][j]);
                    }
                }
            }
        }
        
        int minCost = Integer.MAX_VALUE;
        for (int i = 0; i < colors; i++) {
            minCost = Math.min (dp[houses - 1][i], minCost);
        }
        
        return minCost;
    }
}
```

O(NK)解法，只需要紀錄成本最小的兩個值。

```java
public class Solution {
    public int minCostII(int[][] costs) {
        if (costs == null || costs.length == 0) {
            return 0;
        }
        
        int houses = costs.length;
        int colors = costs[0].length;
        
        int[] dp = new int[colors];
        int minOne = 0;
        int minTwo = 0;
        
        for (int i = 0; i < houses; i++) {
            int oldMinOne = minOne;
            int oldMinTwo = minTwo;
            
            minOne = Integer.MAX_VALUE;
            minTwo = Integer.MAX_VALUE;
            
            for (int j = 0; j < colors; j++) {
                if (dp[j] != oldMinOne || oldMinOne == oldMinTwo) {
                    dp[j] = oldMinOne + costs[i][j];
                } else {
                    dp[j] = oldMinTwo + costs[i][j];
                }
                
                if (minOne <= dp[j]) {
                    minTwo = Math.min(minTwo, dp[j]);
                } else {
                    minTwo = minOne;
                    minOne = dp[j];
                }
            }
        }
        return minOne;
    }
}
```

---
###Reference 
1. http://buttercola.blogspot.com/2015/09/leetcode-paint-house-ii.html