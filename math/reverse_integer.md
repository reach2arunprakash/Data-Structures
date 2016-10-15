# Reverse Integer

[Leetcode](https://leetcode.com/problems/reverse-integer/)


題意：

Reverse digits of an integer.
```
Example1: x = 123, return 321
Example2: x = -123, return -321
```


Have you thought about this?

Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!

If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.

Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?

For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

解題思路：

題目不難，重點是考慮到 overflow的問題，在這裡我們不斷的根據Integer.MAX_VALUE / 10來與當下的 val來作比較，如果當下已經大於MAX_VALUE / 10了，表示待會乘以10時必定會overflow。

程式碼如下：

```java
public class Solution {
    public int reverse(int x) {
        boolean negative = false;
        if (x < 0) {
            negative = true;
        }
        
        int val = 0;
        while (x != 0) {
            if (negative  && val < (Integer.MIN_VALUE / 10)) {
                return 0;
            }
            if (!negative && val > (Integer.MAX_VALUE / 10)) {
                return 0;
            }
            val = val * 10 + x % 10;
            x /= 10;
        }
        
        return val;
    }
}
```