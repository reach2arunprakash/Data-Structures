# Strobogrammatic Number II

[Leetcode](https://leetcode.com/problems/strobogrammatic-number-ii/)

題意：

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

For example,

Given n = 2, return ```["11","69","88","96"]```.

**Hint:**

Try to use recursion and notice that it should recurse with n - 2 instead of n - 1.

解題思路：


使用遞迴去作，由於n可能是奇數或偶數，因此終止條件有0與1兩個，且需要特別處理一下00這個數，此數不是Strobogrammatic number，因此加了個條件當n != m時才在前後加上00


```java
public class Solution {
    public List<String> findStrobogrammatic(int n) {
        return helper(n, n);
    }
    
    private List<String> helper(int n, int m) {
        if (n == 0) {
            return new ArrayList<String>(Arrays.asList(""));
        }
        if (n == 1) {
            return new ArrayList<String>(Arrays.asList("0","1","8"));
        }
        
        List<String> list = helper(n - 2, m);
        List<String> res = new ArrayList<String>();
        for (int i = 0; i < list.size(); i++) {
            String cur = list.get(i);
            
            //重點，防止"00"這種corner case
            if (n != m) {
               res.add("0" + cur + "0"); 
            }
            
            res.add("1" + cur + "1");
            res.add("6" + cur + "9");
            res.add("8" + cur + "8");
            res.add("9" + cur + "6");
        }
        
        return res;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/50412/ac-clean-java-solution