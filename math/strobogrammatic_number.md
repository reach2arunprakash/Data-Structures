# Strobogrammatic Number

[Leetcode](https://leetcode.com/problems/strobogrammatic-number/)

題意：

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

For example, the numbers "69", "88", and "818" are all strobogrammatic.

解題思路：

先把相對應的數字放到hashmap中，再用兩根指針往中間夾，如果遇到不在map中的值，或是map.get(start) != end，則返回false。


```java
public class Solution {
    public boolean isStrobogrammatic(String num) {
        HashMap<Character, Character> map = new HashMap<Character, Character>();
        map.put('0', '0');
        map.put('1', '1');
        map.put('9', '6');
        map.put('6', '9');
        map.put('8', '8');
        
        int start = 0;
        int end = num.length() - 1;
        while (start <= end) {
            char cStart = num.charAt(start);
            char cEnd = num.charAt(end);
            if (!map.containsKey(cStart) || !map.containsKey(cEnd) || map.get(cStart) != cEnd) {
                return false;
            }
            start++;
            end--;
        }
        
        return true;
    }
}
```
