#Longest Common Substring


[原題網址](http://www.lintcode.com/en/problem/longest-common-substring/)

解法：相同斜上再加一，不同則歸零，因不同則歸零，所以不需要初始化第0行與第0列。

```java
public int longestCommonSubstring(String A, String B) {
        
    if (A.length() == 0 || B.length() == 0) {
        return 0;
    }
    
    int[][] result = new int[A.length() + 1][B.length() + 1];
    
    int max = 0;
    for (int i = 1; i <= A.length(); i++) {
        for (int j = 1; j <= B.length(); j++) {
            if (A.charAt(i-1) == B.charAt(j-1)) {
                result[i][j] = result[i-1][j-1] + 1;
                if (result[i][j] > max) {
                    max = result[i][j];
                }
            } else {
                result[i][j] = 0;
            }
        }
    }
    
    return max;
}
```

>Time Complexity：O(n^2)