# Shortest Palindrome

[Leetcode](https://leetcode.com/problems/shortest-palindrome/)

題意：

Given a string S, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

For example:

Given "aacecaaa", return "aaacecaaa".

Given "abcd", return "dcbabcd".

解題思路：


```java
public class Solution {
    public String shortestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        
        int len = s.length();
        int left;
        int right;
        int maxLen = 0;
        for (int i = 0; i < len;) {
            每次從string的中心往兩旁延伸
            left = right = i;
            
            // 往右延伸一樣的數
            while (right < len - 1 && s.charAt(right) == s.charAt(right + 1)) {
                right++;
            }
            
            i = right + 1;
            
            向左右延伸
            while (left > 0 && right < len - 1 && s.charAt(left - 1) == s.charAt(right + 1)) {
                left--;
                right++;
            }
            
            if (left == 0 && maxLen < right + 1) {
                maxLen = right + 1;
            }
        }
        
        String prefix = new StringBuilder(s.substring(maxLen)).reverse().toString();
        
        return prefix + s;
    }
}
```