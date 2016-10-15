# Strobogrammatic Number II

[Leetcode](https://leetcode.com/problems/strobogrammatic-number-ii/)

題意：

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

For example,
Given n = 2, return ["11","69","88","96"].

解題思路：

暴力法：

每個值代進去算，是的話則加入，此法會超時，其程式碼如下：

```java
public class Solution {
    public List<String> findStrobogrammatic(int n) {
        List<String> res = new ArrayList<String>();
        if (n == 0) {
            return res;
        }
        if (n == 1) {
            res.add("0");
        }
        
        int num = 1;
        for (int i = 1; i <= n; i++) {
            num *= 10;
        }
        
        for (int i = num / 10; i < num; i++) {
            String cur = Integer.toString(i);
            if (valid(cur)) {
                res.add(cur);
            }
        }
        
        return res;
    }
    
    public boolean valid(String num) {
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

法二：用遞迴下去作，每次從兩旁不斷延伸，別忘了要加0，其程式碼如下：

```java
public class Solution {
    public List<String> findStrobogrammatic(int n) {
        return helper(n, n);
    }
    
    private List<String> helper(int n, int m) {
        List<String> res = new ArrayList<String>();
        if (n == 0) {
            res.add("");
            return res;
        }
        if (n == 1) {
            res.add("0");
            res.add("1");
            res.add("8");
            return res;
        }
        
        List<String> list = helper(n - 2, m);
        
        for (int i = 0; i < list.size(); i++) {
            String cur = list.get(i);
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