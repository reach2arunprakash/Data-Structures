# Palindrome Number

[Leetcode](https://leetcode.com/problems/palindrome-number/)

題意：

Determine whether an integer is a palindrome. Do this without extra space.



Some hints:

Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.


解題思路：

不斷地取第一位和最後一位（10進制下）進行比較，相等則取第二位和倒數第二位，直到完成比較或者中途找到了不一致的位。

程式碼如下：

```java
public class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0) {
            return false;
        }
        
        int div = 1;
        while (x / div >= 10) {
            div *= 10;
        }
        
        while (x > 0) {
            int l = x / div;
            int r = x % 10;
            if (l != r) {
                return false;
            }
            x = x % div / 10;
            div /= 100;
        }
        
        return true;
    }
}
```
---
###Reference
1. http://fisherlei.blogspot.com/2012/12/leetcode-palindrome-number.html
