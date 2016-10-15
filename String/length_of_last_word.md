#Length of Last Word

[原題網址](http://www.lintcode.com/en/problem/length-of-last-word/)

題意：

Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

**Example**

Given s = "Hello World", return 5.

**Note**

A word is defined as a character sequence consists of non-space characters only.

解題思路：從後面開始，利用 end 變數來紀錄最後一單詞的結尾，為了處理 trailing spaces 的問題，只要遇到space則 end-- ，接著再使用 start 找到單詞的開頭，最後將 end - start 回傳即可，程式碼如下：

```java
public int lengthOfLastWord(String s) {
        
    if (s == null || s.length() == 0) {
        return 0;
    }
    
    int end = s.length() - 1;
    while (end >= 0 && s.charAt(end) == ' ') {
        end--;
    }
    
    int start = end;
    while (start >= 0 && s.charAt(start) != ' ') {
        start--;
    }
    
    return end - start;
}
```

updated 2015.11.19

```java
public class Solution {
    public int lengthOfLastWord(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int count = 0;
        boolean isMeetSpace = false;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == ' ') {
                isMeetSpace = true;
            } else {
               
               if (isMeetSpace) {
                   count = 0;
                   isMeetSpace = false;
               }
               count++;
            }
            
        }
        return count;
    }
}
```