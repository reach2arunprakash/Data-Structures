#Maximum Subarray III

[原題網址](http://www.lintcode.com/en/problem/maximum-subarray-iii/)

題意：

Given an array of integers and a number k, find k non-overlapping subarrays which have the largest sum.

The number in each subarray should be contiguous.

Return the largest sum.

**Example**

Given ```[-1,4,-2,3,-2,3]```, k=2, return 8

**Note**
The subarray should contain at least one number

解題思路：

```dp[i][j]``` 代表的是陣列中前i個元素切成j個子陣列的最大和為多少。


```java
public int maxSubArray(ArrayList<Integer> nums, int k) {
    
    if (nums.size() < k )
        return 0;
        
    int len = nums.size();
    
    int[][] dp = new int[len + 1][k + 1];
    
    for (int i = 1; i <= len; i++) {
        for (int j = 1; j <= k; j++) {
            
            // 代表陣列前i個數太少，不夠切成j個子陣列，因此最大值為0。
            if (i < j) {
                dp[i][j] = 0;
                continue;
            }

            // 以下這段程式碼，主要是枚舉從j-1到i-1之間的最大subarray，
            // dp[i][j] 代表的是陣列中前i個元素切成j個子陣列的最大和為多少
            // 下面作法其實跟Maximum Subarray I一樣
            dp[i][j] = Integer.MIN_VALUE;
            for (int p = j - 1; p <= i - 1; p++) {
                int localMax = nums.get(p);
                int globalMax = localMax;

                for (int t = p + 1; t <= i - 1; t++) {
                    localMax = Math.max(localMax + nums.get(t), nums.get(t));
                    globalMax = Math.max(localMax, globalMax);
                }
                
                if (dp[i][j] < dp[p][j - 1] + globalMax) {
                    dp[i][j] = dp[p][j - 1] + globalMax;
                }
            }
        }
    }
    
    return dp[len][k];
}
```
---
另一網友 [今際の國の呵呵君](http://hehejun.blogspot.com/2015/01/lintcodemaximum-subarray-iii.html) 提出的優化版本：

```localMax[i][j]```為進行了 i 次切割 前 j 個數字的最大子陣列和，並且最後一個子陣列以```A[j - 1]```(A為輸入的數組)結尾。

```java
localMax[i][j] = max(globalMax[i - 1][k] + sumFromTo(A[k + 1], A[j])) 其中 0< k < j;
```


```globalMax[i][j]```為進行了 i 次切割前 j 個數字的最大子陣列和 (最後一個subarray不一定需要以j - 1結尾)。

```java 
globalMax[i][j] = max(globalMax[i][j - 1], localMax[i][j])
```

程式碼如下：

```java
public int maxSubArray(ArrayList<Integer> nums, int k) {
    
    if (nums == null || nums.size() < k) {
        return 0;
    }
    
    int len = nums.size();
    
    // globalMax[i][j] 代表前j個元素切成i份子陣列的最大和
    int[][] globalMax = new int[k + 1][len + 1];
    for (int i = 1; i <= k; i++) {
        int localMax = Integer.MIN_VALUE;
        // 在此優化，j從i-1開始，因若元素個數小於切割數則無法切割
        // 取i-1到len-1是因為陣列範圍的關係，
        
        for (int j = i - 1; j < len; j++) {
            localMax = Math.max(localMax, globalMax[i - 1][j]) + nums.get(j);
            if (j == i - 1) {
                globalMax[i][j + 1] = localMax; // 因無globalMax[i][j]可比較，故直接assign localmax
            } else {
                globalMax[i][j + 1] = Math.max(globalMax[i][j], localMax);
            }
        }
    }
    
    return globalMax[k][len];
}
```
---
###Reference
1. http://hehejun.blogspot.com/2015/01/lintcodemaximum-subarray-iii.html
2. http://www.cnblogs.com/lishiblog/p/4183917.html