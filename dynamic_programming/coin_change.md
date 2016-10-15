# Coin Change

[Leetcode](https://leetcode.com/problems/coin-change/)

題意：

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

Example 1:
coins = [1, 2, 5], amount = 11
return 3 (11 = 5 + 5 + 1)

Example 2:
coins = [2], amount = 3
return -1.

Note:
You may assume that you have an infinite number of each kind of coin.

解題思路：

使用dp來解

> dp[i] 代表目前現有的coins能換成i的最少張數
> dp[i] = min(dp[i - coin[j]) + 1;

```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        
        if (coins == null || coins.length == 0 || amount == 0) {
            return 0;
        }
        
        int[] dp = new int[amount + 1];
        dp[0] = 0;
        for (int i = 1; i <=  amount; i++) {
            
            dp[i] = Integer.MAX_VALUE;
            
        }
        
        for (int i = 1; i <= amount; i++) {
            for (int j = 0; j < coins.length; j++) {
                //要注意是否該dp無法換，即等於Integer.MAX_VALUE
                if (coins[j] > i || dp[i - coins[j]] == Integer.MAX_VALUE) {
                    continue;
                }
                
                int cur = dp[i - coins[j]] + 1;
                dp[i] = Math.min(dp[i], cur);
            }
        }
        
        return (dp[amount] == Integer.MAX_VALUE) ? -1 : dp[amount];
    }
}
```