# Length of Last Word

[Leetcode](https://leetcode.com/problems/length-of-last-word/)


題意：

Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

For example, 

Given s = "Hello World",

return 5.

解題思路：

從後面往前找，找到第一個空格之後，即返回len，否則len++，字串裡沒空格也沒關係，他會遇到字串頭即跳出for loop，最後再返回len。

```java
public class Solution {
    public int lengthOfLastWord(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int len = 0;
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s.charAt(i) != ' ') {
                len++;
            }
            
            if (s.charAt(i) == ' ' && len != 0) {
                return len;
            }
        }
        return len;
    }
}
```
---
###Reference
1. http://www.cnblogs.com/springfor/p/3872326.html