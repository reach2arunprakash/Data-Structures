# Strobogrammatic Number III

[Leetcode](https://leetcode.com/problems/strobogrammatic-number-iii/)

題意：

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to count the total strobogrammatic numbers that exist in the range of low <= num <= high.

For example,
Given low = "50", high = "100", return 3. Because 69, 88, and 96 are three strobogrammatic numbers.

Note:
Because the range might be a large number, the low and high numbers are represented as string.


解題思路：

主要還是用到2的整個template，網友 [czonzhu](https://leetcode.com/discuss/55468/clear-java-ac-solution-using-strobogrammatic-number-method) 提供以下思路：

"The basic idea is to find generate a list of strobogrammatic number with the length between the length of lower bound and the length of upper bound. Then we pass the list and ignore the numbers with the same length of lower bound or upper bound but not in the range."

```java
public class Solution {
    public int strobogrammaticInRange(String low, String high) {
        int count = 0;
        List<String> res = new ArrayList<String>();
        for (int n = low.length(); n <= high.length(); n++) {
            res.addAll(helper(n,n));
        }
        
        for (String num : res) {
            if ((num.length() == low.length() && num.compareTo(low) < 0) ||
                (num.length() == high.length() && num.compareTo(high) > 0)) {
                    continue;
                }
                count++;
        }
        return count;
    }
    
    private List<String> helper(int cur, int max) {
        if (cur == 0) {
            return new ArrayList<String>(Arrays.asList(""));
        }
        
        if (cur == 1) {
            return new ArrayList<String>(Arrays.asList("0", "1", "8"));
        }
        
        List<String>  res = new ArrayList<String>();
        List<String> list = helper(cur - 2, max);
        
        for (int i = 0; i < list.size(); i++) {
            String tmp = list.get(i);
            if (cur != max) {
                res.add("0" + tmp + "0");
            }
            res.add("1" + tmp + "1");
            res.add("6" + tmp + "9");
            res.add("8" + tmp + "8");
            res.add("9" + tmp + "6");
            
        }
        
        return res;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/55468/clear-java-ac-solution-using-strobogrammatic-number-method