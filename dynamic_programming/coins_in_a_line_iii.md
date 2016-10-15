#Coins in a Line III

[原題網址](http://www.lintcode.com/en/problem/coins-in-a-line-iii/)

題意：給定兩個陣列，兩人輪流取硬幣，每次只能取一個，從左邊或右邊任取一個硬幣，誰取得多誰贏。

解題思路：使用二維的動態規劃來幫助我們解決這道問題。

>f[x][y]，第x個硬幣到y的硬幣，現在先手取硬幣的人最後最多取硬幣。


```java
public class Solution {
    /**
     * @param values: an array of integers
     * @return: a boolean which equals to true if the first player will win
     */
    int len;
    int[][] dp;
    public boolean firstWillWin(int[] values) {
        
        if (values == null || values.length == 0) {
            return false;
        }
        
        len = values.length;
        dp = new int[len][len];
        int sum = 0;
        // initialize array
        for (int[] line : dp) {
            Arrays.fill(line, -1);
        }
        
        for (int i = 0; i < len; i++) {
            sum += values[i];
        }
        
        int maxProfit = memorySearch(0, len - 1, values);
        
        if (maxProfit > sum / 2) {
            return true;
        } else {
            return false;
        }
        
    }
    
    private int memorySearch(int m, int n, int[] values) {
        
        if (dp[m][n] != -1) {
            return dp[m][n];
        } else if (m == n) {
            dp[m][n] = values[m];
        } else if (Math.abs(m-n) == 1) {
            dp[m][n] = Math.max(values[m], values[n]);
        } else {
            int takeLeft = Math.min(memorySearch(m + 2, n, values), memorySearch(m + 1, n - 1, values)) + values[m];
            int takeRight = Math.min(memorySearch(m + 1, n - 1, values), memorySearch(m, n - 2, values)) + values[n];
            
            dp[m][n] = Math.max(takeLeft, takeRight);
        }
        
        return dp[m][n];
    }
}

```
>Time Complexity：$$O(N^{2})$$