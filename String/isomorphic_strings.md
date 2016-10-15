# Isomorphic Strings

[Leetcode](https://leetcode.com/problems/isomorphic-strings/)

題意：

Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

For example,
Given "egg", "add", return true.

Given "foo", "bar", return false.

Given "paper", "title", return true.


解題思路：

使用hashmap來幫助我們解決這道題，不斷的把s字元對應到相對應的t字元，一但發現不同時，即返回false，最後還需要檢查是否value的數目與key的數目相同。為了避免"ab"與"ab"的問題，其程式碼如下：

```java
public class Solution {
    public boolean isIsomorphic(String s, String t) {
        HashMap<Character, Character> map = new HashMap<Character, Character>();
        if (s == null || t == null || s.length() != t.length()) {
            return false;
        }
        
        for (int i = 0; i < s.length(); i++) {
            char cS = s.charAt(i);
            char cT = t.charAt(i);
            if (!map.containsKey(cS)) {
                map.put(cS, cT);
            } else {
                if (map.get(cS) != cT) {
                    return false;
                }
            }
        }
        
        return map.values().size()== new HashSet<>(map.values()).size();
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/68132/simple-java-solution-clean-code