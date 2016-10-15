#Post Office Problem

[Lintcode](http://www.lintcode.com/en/problem/post-office-problem/)

題意：

給出m個村莊及其距離，給出n個郵局，要求怎麼建n個郵局使代價最小.

On one line there are n houses. Give you an array of integer means the position of each house. Now you need to pick k position to build k post office, so that the sum distance of each house to the nearest post office is the smallest. Return the least possible sum of all distances between each village and its nearest post office.

Example

Given array ```a = [1,2,3,4,5]```, k = 2. return ```3```.

Challenge

Could you solve this problem in O(n^2) time ?


解題思路：

我們可以利用動態規劃來幫助我們解這題，網友 [find_my_dream](http://blog.csdn.net/find_my_dream/article/details/4931222) 提供了以下思路：

>dis[i][j] 表示在第i個村莊與第j個房子中間造一間郵局的情況下，所有村莊到郵局的距離和

計算dis[i][j]的方式：

1. 先求得i 與 j 的中點 mid = (i + j) / 2
2. 接著把所有i 與 j 中間所有村莊與郵局的距離加總起來即可

>dp[i][l] 表示在前i個村莊建l間郵局的最小距離和

計算dp[i][l] 的方式：

1. 從第 1 座村莊到第 i 座村莊找出一個切點 j 使得滿足以下條件
    1. 前 j 個村莊蓋 l - 1 間郵局，加上在第 j + 1 個村莊到第 i 個村莊的中間蓋一間郵局的矩離和最小。
    2. dp[i][l] = dp[j][l - 1] + dis[j + 1][i]




```java
public class Solution {
    /**
     * @param A an integer array
     * @param k an integer
     * @return an integer
     */
    public int postOffice(int[] A, int k) {
        
        // 如果沒有半間房，或是郵局的數量比房子多的話，直接返回0
        if (A == null || A.length == 0 || k >= A.length) {
            return 0;
        }
        
        int len = A.length;
        Arrays.sort(A);
        int[][] dp = new int[len + 1][len + 1];
        int[][] dis = init(A);
        
        // 前i間房只能蓋一間郵局的話，就不斷的把郵局蓋在前i間房的中間
        for (int i = 0; i <= len; i++) {
            dp[i][1] = dis[1][i];
        }
        
        //要蓋兩間房的話才有戲唱，因此從兩間房開始枚舉
        for (int l = 2; l <= k; l++) {
            for (int i = l; i <= len; i++) {
                dp[i][l] = Integer.MAX_VALUE;
                for (int j = 0; j < i; j++) {
                    if (dp[i][l] == Integer.MAX_VALUE || dp[i][l] > (dp[j][l - 1] + dis[j + 1][i])) {
                        dp[i][l] = dp[j][l - 1] + dis[j + 1][i];
                    } 
                }
            }
        }
        
        return dp[len][k];
        
    }
    
    private int[][] init(int[] A) {
        
        int len = A.length;
        int[][] dist = new int[len + 1][len + 1];
        
        for (int i = 1; i <= len; i++) {
            for (int j = i + 1; j <= len; j++) {
                int mid = (i + j) / 2;
                for (int k = i; k <= j; k++) {
                    dist[i][j] += Math.abs(A[k - 1] - A[mid - 1]);
                }
            }
        }
        
        return dist;
    }
}
```

---
###Reference
1. http://blog.csdn.net/find_my_dream/article/details/4931222