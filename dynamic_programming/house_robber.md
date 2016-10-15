#House Robber

[原題網址](http://www.lintcode.com/en/problem/house-robber/#)

題意：給予一陣列，陣列中每一元素代表每個家的財產，小偷不能連續偷兩間房，求最大獲利。

解題思路：使用動態規劃來作，程式碼如下：

> dp[i]代表從第0間到第i間小偷的最大獲利為多少，即比較偷當前這間的最大獲利，與不偷當前這間哪間獲利較大，狀態轉換如下

> dp[i] = max(dp[i - 1], dp[i - 2] + dp[i]);

```java
public long houseRobber(int[] A) {
    
    if (A == null || A.length <= 3) {
        if (A == null || A.length == 0) {
            return 0;
        } else if (A.length == 1) {
            return A[0];
        } else if (A.length == 2) {
            return Math.max(A[0], A[1]);
        } else if (A.length == 3) {
            return  Math.max((A[0] + A[2]), A[1]);
        }
    }
    
    int len = A.length;
    long[] profit = new long[len];
    profit[0] = A[0];
    profit[1] = Math.max(A[0], A[1]);
    profit[2] = Math.max((A[0] + A[2]), A[1]);
    
    for (int i = 3; i < len; i++) {
        profit[i] = Math.max(profit[i-1], profit[i-2] + A[i]);
    }
    
    return profit[len-1];
}
```

>Time Complexity：$$O(N)$$，只需遍歷一次陣列即可。

>Space Complexity： $$O(N)$$，此可用滾動陣列來優化到 $$O(1)$$。

---
解法二：使用動態規劃加遞迴來作，但會耗用太多 stack 空間，解法如下：

```java
public class Solution {
    /**
     * @param A: An array of non-negative integers.
     * return: The maximum amount of money you can rob tonight
     */
    public long houseRobber(int[] A) {
        
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int len = A.length;
        long[] dp = new long[len];
        long max = 0;
        
        Arrays.fill(dp, -1);
        memorySearch(A, dp, len - 1);
        
        return dp[len - 1];
    }
    
    private long memorySearch(int[] A, long dp[], int i) {
        
        if (i < 0) {
            return 0;
        }
        
        if (dp[i] != -1) {
            return dp[i];
        }
        
        if (i == 0) {
            dp[0] = A[0];
        } else if (i == 1) {
            dp[1] = Math.max(A[0], A[1]);
        } else if (i == 2) {
            dp[2] = Math.max(A[0] + A[2], A[1]);
        } else {
            dp[i] = Math.max(memorySearch(A, dp, i - 2), memorySearch(A, dp, i - 3)) + A[i];
        }
        
        return dp[i];
    }
}
```

updated 2015.11.29

後來到一個神奇的解法，只需要O(1)的空間，其實可以轉化成奇偶問題，因為不能連續搶，其程式碼如下：

```java
public class Solution {
    public int rob(int[] nums) {
        int len = nums.length;
        int odd = 0;
        int even = 0;
        for (int i = 0; i < len; i++) {
            if (i % 2 == 0) {
                odd = Math.max(odd + nums[i], even);
            } else {
                even = Math.max(odd, even + nums[i]);
            }
        }
        
        return Math.max(even ,odd);
    }
}
```