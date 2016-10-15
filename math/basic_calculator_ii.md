# Basic Calculator II

[Leetcode](https://leetcode.com/problems/basic-calculator-ii/)

題意：

mplement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid.

Some examples:
```
"3+2*2" = 7
" 3/2 " = 1
" 3+5 / 2 " = 5
```


解題思路：


```java
public class Solution {
    public int calculate(String s) {
        if (s == null) {
            return 0;
        }
        
        s = s.trim().replaceAll(" +", "");
        int len = s.length();
        
        int res = 0;
        long preVal = 0;
        char sign = '+';
        int i = 0;
        while (i < len) {
            long curVal = 0;
            while (i < len && (int)s.charAt(i) >= 48 && (int)s.charAt(i) <= 57) {
                curVal = curVal * 10 + (s.charAt(i) - '0');
                i++;
            }
            
            if (sign == '+') {
                res += preVal;
                preVal = curVal;
            } else if (sign == '-') {
                res += preVal;
                preVal = -curVal;
            } else if (sign == '*') {
                preVal = preVal * curVal;
            } else if (sign == '/') {
                preVal = preVal / curVal;
            }
            
            if (i < len) {
                sign = s.charAt(i);
                i++;
            }
        }
        
        res += preVal;
        return res;
    }
}
```