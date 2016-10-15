#Longest Palindrome Substring

[原題網址](http://www.lintcode.com/en/problem/longest-palindromic-substring/)

題意：從字串 S 中找出最長的迴文

Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.

**Example**

Given the string = "abcdzdcab", return "cdzdc".

**Challenge**

O(n2) time is acceptable. Can you do it in O(n) time.

解題思路：可以使用動態規劃來幫助我們解決這道問題，

$$O(N^{2})$$解法：枚舉子字串的起始點與終點，dp[i][j]代表從字串取出 i 到 j 的子字串是否為迴文，可以透過比較第i個字元是否與第j個字元相同，並且兩者之間的子字串是否為迴文。

>```dp[i][j] = (s.charAt(i) == s.charAt(j)) && dp[i + 1][j - 1]```

原始碼如下：

```java
public String longestPalindrome(String s) {
    
    if (s == null || s.length() < 2) {
        return s;
    }
    
    int len = s.length();
    boolean[][] dp = new boolean[len][len];
    String res = "";
    int maxLength = 0;
    
    // j枚舉終點，i枚舉超始點
    // subString(i,j)表示從string的index i取到j-1，即不包含j本身。
    for (int j = 0; j < len; j++) {
        for (int i = 0; i <= j; i++) {
            
            dp[i][j] = (s.charAt(i) == s.charAt(j)) && ( j - i < 2 || dp[i+1][j-1]);
            
            if (dp[i][j]) {
                if (j - i + 1 > maxLength) {
                    maxLength = j - i + 1;
                    res = s.substring(i, j + 1);
                }
            }
        }
    }
    
    return res;
}
```
---
另一解法，由每個字元為中心，不斷向兩旁延伸，考慮奇數長度與偶數長度。

```java
public class Solution {
    public String longestPalindrome(String s) {
        
        if (s == null || s.length() <= 1) {
            return s;
        }
        
        // 取第一個字元
        String res = s.substring(0, 1);
        for (int i = 0; i < s.length(); i++) {
            
            //考慮奇數長度
            String temp = helper(s, i, i);
            if (temp.length() > res.length()) {
                res = temp;
            }
            
            //考慮偶數長度
            temp = helper(s, i, i+ 1);
            if (temp.length() > res.length()) {
                res = temp;
            }
        }
        
        return res;
    }
    
    private String helper(String s, int start, int end) {
        while ((start >= 0) && (end <= s.length() - 1) && (s.charAt(start) == s.charAt(end))) {
            start--;
            end++;
        }
        //考慮substring的特性，雖然start與end已改變，但修正回來上一個成立的子字串
        return s.substring(start + 1, end);
    }
}
```

---
###Reference
1. http://www.cnblogs.com/yuzhangcmu/p/4189068.html
2. http://blog.csdn.net/hopeztm/article/details/7932245
3. http://bangbingsyb.blogspot.com/2014/11/leetcode-longest-palindromic-substring.html
4. http://articles.leetcode.com/2011/11/longest-palindromic-substring-part-i.html
5. http://www.programcreek.com/2013/12/leetcode-solution-of-longest-palindromic-substring-java/

