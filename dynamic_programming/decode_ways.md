# Decode Ways

[Leetcode](https://leetcode.com/problems/decode-ways/)

題意：

A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26

Given an encoded message containing digits, determine the total number of ways to decode it.

For example,

Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2.


解題思路：

此題可以利用[Climb Stairs]() 的動態規劃方法來幫助我們解這道題，我們另外需要兩個function來幫助我們驗證是否正確的編碼，分別驗證一個char與兩個char，遞迴公式如下

>nums[i] += nums[i - 1] if checkOne(s.charAt(i)) == true
>nums[i] += nums[i - 1] + nums[i - 2] if checkOne(s.charAt(i)) == true && checkTwo(s.charAt(i - 1), s.charAt(i - 2))

程式碼如下：

```java
public class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0 || s.charAt(0) == '0') {
            return 0;
        }
        if (s.length() == 1) {
            return 1;
        }
        
        int len = s.length();
        int[] nums = new int[len];
        nums[0] = checkOne(s.charAt(0));
        
        // 乘以nums[0]是要為了預防01 這種不合法的編碼
        nums[1] = (checkTwo(s.charAt(0), s.charAt(1))) + checkOne(s.charAt(1)) * nums[0];
        for (int i = 2 ; i < len; i++) {
            if (checkOne(s.charAt(i)) == 1) {
                nums[i] += nums[i - 1];
            }
            if (checkTwo(s.charAt(i - 1), s.charAt(i)) == 1) {
                nums[i] += nums[i - 2];
            }
            if (nums[i] == 0) {
                return 0;
            }
        }
        
        return nums[len - 1];
        
    }
    
    
    private int checkOne(char c) {
        return (c == '0') ? 0 : 1;
    }
    
    private int checkTwo(char a, char b) {
        if (a == '1' || (a == '2' && b <= '6')) {
            return 1;
        }
        return 0;
    }
}
```
---
###Reference
1. http://fisherlei.blogspot.com/2013/01/leetcode-decode-ways-solution.html