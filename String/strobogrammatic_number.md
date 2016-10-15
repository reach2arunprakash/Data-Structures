# Strobogrammatic Number

[Leetcode](https://leetcode.com/problems/strobogrammatic-number/)

題意：

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

For example, the numbers "69", "88", and "818" are all strobogrammatic.

解題思路：

用hashmap先建立對應關係，再用檢查palindrome的方式來一一檢查，程式碼如下：

```java
public class Solution {
    public boolean isStrobogrammatic(String num) {
        HashMap<Character, Character> map = new HashMap<Character, Character>();
        map.put('0', '0');
        map.put('1', '1');
        map.put('6', '9');
        map.put('8', '8');
        map.put('9', '6');
        
        int start = 0;
        int end = num.length() - 1;
        while (start <= end) {
            char sChar = num.charAt(start);
            char eChar = num.charAt(end);
            if (!map.containsKey(sChar) || !map.containsKey(eChar) || map.get(sChar) != eChar) {
                return false;
            }
            start++;
            end--;
        }
        
        return true;
    }
}
```