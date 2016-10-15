# Reverse Words in a String

[Leetcode](https://leetcode.com/problems/reverse-words-in-a-string/)

題意：

Given an input string, reverse the string word by word.

For example,
Given s = "the sky is blue",
return "blue is sky the".

Update (2015-02-12):
For C programmers: Try to solve it in-place in O(1) space.

click to show clarification.

Clarification:
What constitutes a word?
A sequence of non-space characters constitutes a word.
Could the input string contain leading or trailing spaces?
Yes. However, your reversed string should not contain leading or trailing spaces.
How about multiple spaces between two words?
Reduce them to a single space in the reversed string.

解題思路：

先把整個字串反轉過來，再反轉每個word即可。

```java
public class Solution {
    public String reverseWords(String s) {
        if (s == null || s.length() == 0) {
            return s;
        }
        String formatted = s.trim().replaceAll(" +", " ");
        if (formatted.length() < 2) {
            return formatted;
        }
        
        char[] charArray = formatted.toCharArray();
        reverse(charArray, 0, charArray.length - 1);
        int j = 0;
        for (int i = 0; i < charArray.length; i++) {
            if (charArray[i] == ' ') {
                reverse(charArray, j, i - 1);
                j = i + 1;
            } else if (i == charArray.length - 1) {
                reverse(charArray, j, i);
            }
        }
        return new String(charArray);
    }
    
    private void reverse(char[] charArray, int start, int end) {
        while (start <= end) {
            char temp = charArray[start];
            charArray[start] = charArray[end];
            charArray[end] = temp;
            start++;
            end--;
        } 
    }
}
```


updated 2016.1.19
用 stringbuilder
```java
public class Solution {
    public String reverseWords(String s) {
        StringBuilder reversed = new StringBuilder();
        int j = s.length();
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s.charAt(i) == ' ') {
                j = i;
            } else if (i == 0 || s.charAt(i - 1) == ' ') {
                if (reversed.length() != 0) {
                    reversed.append(" ");
                }
                reversed.append(s.substring(i, j));
            }
        }
        
        return reversed.toString();
    }
}
```