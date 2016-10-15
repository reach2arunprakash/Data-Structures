#Coins in a Line II

[原題網址](http://www.lintcode.com/en/problem/coins-in-a-line-ii/)

題意：與 [Coins in a Line](dynamic programming/coins_in_a_line.md) ，一樣的規則，唯一不同則是每個硬幣有價值，判斷先手是否會贏。

解題思路：此為計數型的動態規劃，我們能作的就是找出先手先取的最大獲利數，接著判斷獲利數是否大於總數的二分之一，若是則贏，否則輸，程式碼如下：

[優化]使用記憶化搜尋來避免重複計算的問題

**dp[n]：代表目前還剩下n個硬幣，而先手先取的最大獲利**

```java
public class Solution {
    /**
     * @param values: an array of integers
     * @return: a boolean which equals to true if the first player will win
     */
     
    int[] dp;
    int len;
    public boolean firstWillWin(int[] values) {
        
        if (values == null || values.length == 0) {
            return false;
        }
        
        len = values.length;
        dp = new int[len + 1];
        Arrays.fill(dp, -1);
        
        int sum = 0;
        for (int i = 0; i < len; i++) {
            sum += values[i];
        }
        
        int res = memorySearch(len, values);
        
        if (res > sum / 2) {
            return true;
        } else {
            return false;
        }
    }
    
    private int memorySearch(int n, int[] values) {

        if (dp[n] != -1) {
            return dp[n];
        } else if (n == 0) {
            dp[n] = 0; // 沒硬幣可拿，獲利0
        } else if (n == 1) {
            dp[n] = values[len - 1]; // 剩下一個硬幣，拿最後那個
        } else if (n == 2) {
            dp[n] = values[len - 2] + values[len - 1]; // 剩下兩個硬幣，拿最後兩個
        } else if (n == 3) {
            dp[n] = values[len - 2] + values[len - 3]; // 剩下三個硬幣，拿兩個，最後一個留給後手
        } else {
            // 找出先手拿一個硬幣的最大獲利
            int firstTakeOne = Math.min(memorySearch(n-2, values), memorySearch(n-3, values)) 
                                + values[len - n];
            // 找出先手拿二個硬幣的最大獲利，記得第二個硬幣是len - n + 1
            int firstTakeTwo = Math.min(memorySearch(n-3, values), memorySearch(n-4, values)) 
                                + values[len - n] + values[len - n + 1];
            dp[n] = Math.max (firstTakeOne, firstTakeTwo);
                    
        }
        return dp[n];
    }
}

```
>Time Complexity：O(N)