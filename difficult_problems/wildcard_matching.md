# Wildcard Matching

[Lintcode](http://www.lintcode.com/en/problem/wildcard-matching/)

題意：

Implement wildcard pattern matching with support for '?' and '*'.

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).

Example
```
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```

解題思路：

DP 解法，在Leetcode上無法通過最後一個case，會超時：

網友[今際の國の呵呵君](http://hehejun.blogspot.com/2014/11/leetcodewildcard-matching.html) 提供了以下思路：

....../a, ....../c 兩個char不匹配，則賦值false即可。

....../a, ....../a 或者 ....../a, ....../? 兩個char匹配，則看前面有沒有match，即[i, j] = [i - 1, j - 1]

....../a, ....../* 此情況最難處理

首先如果[i, j - 1]匹配，[i, j]自然也匹配。如果[i - 1 , j ]匹配，那麼[i , j]也匹配。

再者，如果[i - 1, j - 1]匹配，[i, j]也匹配(但這種情況我們不需要考慮因為，如果[i - 1, j - 1]匹配，那麼[i - 1 , j ]肯定匹配)。

其程式碼如下：

```java
public class Solution {
    /**
     * @param s: A string 
     * @param p: A string includes "?" and "*"
     * @return: A boolean
     */
    public boolean isMatch(String s, String p) {
        
        if (s == null || p == null) {
            return false;
        }
        
        //isMatch[i][j] 表示p的前i個字元，是否與s的前j個字元match
        int lenP = p.length();
        int lenS = s.length();
        boolean[][] isMatch = new boolean[lenP + 1][lenS + 1];
        isMatch[0][0] = true;
        for (int i = 0; i < lenP; i++) {
            //在這裡計算了p個前i個字元是否能match上空字串
            isMatch[i + 1][0] = (p.charAt(i) == '*') ? isMatch[i][0] : false;
        }
        
        for (int i = 0; i < lenP; i++) {
            for (int j = 0; j < lenS; j++) {
                if (p.charAt(i) == '*') {
                    // 因為星號，所以主要考慮上面提到的三個case
                    isMatch[i + 1][j + 1] = isMatch[i][j + 1] || isMatch[i + 1][j];
                } else if (p.charAt(i) == s.charAt(j) || p.charAt(i) == '?') {
                    // 若過當前p字元為?或是字元有match到，則照抄之前是否有match
                    isMatch[i + 1][j + 1] = isMatch[i][j];
                } else {
                    兩者不match，直接放入false
                    isMatch[i + 1][j + 1] = false;
                }
            }
        }
        
        return isMatch[lenP][lenS];
     }
}

```
>Time Complexity： O(M*N)