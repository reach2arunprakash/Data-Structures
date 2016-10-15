# Valid Palindrome

[Leetcode](https://leetcode.com/problems/valid-palindrome/)

題意：

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.

Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.


解題思路：

只比較數字與字母，用兩根指針向中間夾擊，程式碼如下：

```java
public class Solution {
    public boolean isPalindrome(String s) {
        if (s == null || s.length() < 2) {
            return true;
        }
        
        int left = 0;
        int right = s.length() - 1;
        while (left <= right) {
            while (left <= right && !Character.isLetter(s.charAt(left)) && !Character.isDigit(s.charAt(left))) {
                left++;
            }
            while (left <= right && !Character.isLetter(s.charAt(right)) && !Character.isDigit(s.charAt(right))) {
                right--;
            }
            if (left <= right) {
                char leftChar = Character.toLowerCase(s.charAt(left));
                char rightChar = Character.toLowerCase(s.charAt(right));
                if (leftChar != rightChar) {
                    return false;
                }
                left++;
                right--;
            }
            
        }
        return true;
    }
}
```