#Coins in a Line

[]()

題意：一條線上有 n 個硬幣，有兩個人輪流取，每次可取兩個或一個，取到最後一個硬幣的人贏。

解題思路：以先手的角度來判斷是否會贏。 f[x] 表示現在剩下 x 個硬幣，現在先手先取硬幣的最後輸贏的狀況。

初始化
1. f[1] = true，先手直接取一個，贏
2. f[2] = true，先手直接取兩個，贏
3. f[3] = false，先手取一個，後手取兩個，輸。先手取兩個，後手取一個，輸。
4. f[4] = true，先手取一個，後手取一個，先手取兩個，贏。先手取一個。
5. f[5] = true，後手取兩個，先手再取一個，贏。


遞迴式： ``` f[n] = (f[n-2] && f[n-3]) || (f[n-3] && f[n-4])```

程式如下，但以下程式會不斷重複計算之前算過的值，會超時，時間複雜度為指數。

```java
public boolean firstWillWin(int n) {
        
    if (n <= 5) {
        boolean result = true;
        if (n == 3) {
            result = false;
        }
        return result;
    } else {
        return firstWillWin(n-2) && firstWillWin(n-3) || 
                firstWillWin(n-3) && firstWillWin(n-4);
    }
    
}
```

另有記憶化搜尋以一個陣列來紀錄之前算過的值，就是以動態規畫的方式來保存之前算過的值，下次再遇到時直接返回即可。

```java
public class Solution {
    /**
     * @param n: an integer
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int n) {
        
        int[] dp = new int[n + 1];
        return memorySearch(n, dp);
    }
    
    private boolean memorySearch(int n, int[] dp) {
        // 0 : empty, 1: true, 2: false
        if (dp[n] != 0) {
            return dp[n] == 1 ? true : false;
        }
        
        if (n <= 5) {
            dp[n] = 1;
            if (n== 0 || n == 3) {
                dp[n] = 2;
            }
        } else {
            boolean result = memorySearch(n-2, dp) && memorySearch(n-3, dp) ||
                            memorySearch(n-3, dp) && memorySearch(n-4, dp);
            if (result == true) {
                dp[n] = 1;
            } else {
                dp[n] = 2;
            }
        }
        
        return dp[n] == 1 ? true : false;
    }
    
}
```

---
###Reference
1. http://www.jiuzhang.com/solutions/coins-in-a-line/