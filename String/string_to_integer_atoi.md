# String to Integer (atoi)

[Lintcode](http://www.lintcode.com/en/problem/string-to-integeratoi/)

題意：

Implement function atoi to convert a string to an integer.

If no valid conversion could be performed, a zero value is returned.

If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.

Have you met this question in a real interview? Yes
Example
"10" => 10

"-1" => -1

"123123123123123" => 2147483647

"1.0" => 1



解題思路：

updated on 2016.1.9

```java
public class Solution {
    public int myAtoi(String str) {
        boolean isPositive = true;
        int maxDiv10 = Integer.MAX_VALUE / 10;
        int val = 0;
        int i = 0;
        
        while (i < str.length() && str.charAt(i) == ' ') {
            i++;
        }
        
        if (i < str.length() && (str.charAt(i) == '+' || str.charAt(i) == '-')) {
            if (str.charAt(i) == '-') {
                isPositive = false;
            }
            i++;
        }
        
        while (i < str.length() && Character.isDigit(str.charAt(i))) {
            int digit = str.charAt(i) - '0';

            if (val > maxDiv10 || val == maxDiv10 && digit >= 8) {
                return isPositive ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            }
            
            val = val * 10 + digit;
            i++;
        }
        
        if (!isPositive) {
            return -val;
        } else {
            return val;
        }
    }
}
```

不知為什麼 Lintcode 把這題分到 hard，在leetcode是 easy。

照著數字的特性寫即可，記得忽略前面的空白，與判斷是否有正負號來決定數字是正還是負的。

用以下判斷式來決定是否有溢位
>result > maxDiv10 || result == maxDiv10 && digit >= 8

程式碼如下：

```java
public class Solution {
    /**
     * @param str: A string
     * @return An integer
     */
     
    private static final int maxDiv10 = Integer.MAX_VALUE / 10;
    
    public int atoi(String str) {
        
        if (str == null || str.length() == 0) {
            return 0;
        }
        
        long result = 0;
        boolean isNegative = false;
        int pos = 0;
        while(Character.isWhitespace(str.charAt(pos))) {
            pos++;
        } 
        
        if (str.charAt(pos) == '-' || str.charAt(pos) == '+') {
            isNegative = str.charAt(pos) == '-' ? true : false;
            pos++;
        }
        while(pos < str.length() && Character.isDigit(str.charAt(pos))) {
            int digit = Character.getNumericValue(str.charAt(pos));
            result *= 10;
            result += digit;
            
            //overflow handling
            if (result > maxDiv10 || result == maxDiv10 && digit >= 8) {
                return isNegative ? Integer.MIN_VALUE : Integer.MAX_VALUE;
            }
            
            pos++;
        }
        
        if (isNegative) {
            result = 0 - result;
        }
        
        return (int)result;
    }
}

```

---
###Reference
1.http://zolibra.github.io/2014/11/25/leetcodestring-to-integer/