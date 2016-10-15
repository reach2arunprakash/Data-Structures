#Dynamic Programming
[原題連結](http://www.lintcode.com/en/problem/edit-distance/)

解題思路：比較第一個字串的第 i 個字元，與第二個字串的第 j 個字元，會以下兩種情況

若兩者不同：

1. 替換：把當前第一個字串的第 i 個字元替換成第二個字串的第 j 個字元，繼續考慮 f[i-1][j-1]
2. 增加：在第一個字串的第 i 個字元後面增加一個字元，繼續考慮f[i-1][j]
3. 刪除：直接刪除第一個字串的第 i 個字元，且考慮 f[i][j-1]

若兩者相同(因當前字元不需對在一起，因此仍有以下三個選項)：
1. 替換：不需要
2. 增加：在第一個字串的第 i 個字元後面增加一個字元，繼續考慮f[i-1][j]
3. 刪除：直接刪除第一個字串的第 i 個字元，且考慮 f[i][j-1]



```java
public int minDistance(String word1, String word2) {
    // write your code here
    int m = word1.length();
    int n = word2.length();
    
    if (m == 0) {
        return n;
    }
    if (n == 0) {
        return m;
    }
    
    int[][] res = new int[m+1][n+1];
    for (int i = 0; i <= m; i++) {
        res[i][0] = i;
    }
    for (int j = 0; j <= n; j++) {
        res[0][j] = j;
    }
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i-1) != word2.charAt(j-1)) {
                //res[i-1][j]，把第一個string的第i個char刪掉，
                //res[i][j-1]，把加入char到第一個string的第i個位置來match第二個string的第j個char
                // 因此要確保前i個位置與前j-1個位置值一樣
                //res[i-1][j-1]，直接replace，因此確保兩個string的前面char都一樣
                res[i][j] = Math.min(res[i-1][j], Math.min(res[i][j-1], res[i-1][j-1])) + 1;
            } else {
                //因為都一樣，因此只要確保add跟delete後的成本比什麼都不變來得低
                res[i][j] = Math.min(res[i-1][j-1], Math.min(res[i][j-1] + 1, res[i-1][j-1] + 1));
            }
        }
    }
    return res[m][n];
}
```
>Time Complexity：O($$N^{2}$$)


