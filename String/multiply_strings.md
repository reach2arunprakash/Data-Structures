# Multiply Strings

[Leetcode](https://leetcode.com/problems/multiply-strings/)

題意：

解題思路：

先處理各個位數的總乘積和，接著再處理進位的部份，res[i + j] = s[i] * s[j]，記得要處理leading zeros的問題，由於index與位數是相返，需要作反序的處理。

如 98 * 81

res[0] = 8 * 1

res[1] = 9 * 1 + 8 * 8

res[2] = 9 * 8

res[3] = 0

接著再處理進位的問題並將結果放到sb的最前面，因越前面代表越高位。

程式碼如下

```java
public class Solution {
    public String multiply(String num1, String num2) {
        if (num1 == null || num2 == null) {
            return "";
        }
        
        // 先把字串透過stringbuilder反轉過來。
        num1 = new StringBuilder(num1).reverse().toString();
        num2 = new StringBuilder(num2).reverse().toString();
        int[] res = new int[num1.length() + num2.length()];
        
        
        for (int i = 0; i < num1.length(); i++) {
            int a = num1.charAt(i) - '0';
            for (int j = 0; j < num2.length(); j++) {
                int b = num2.charAt(j) - '0';
                res[i + j] += a * b;
            }
        }
        
        //再處理進位的部份，由於之讀反序處理，在這裡我們從頭處理把數值反回來
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < res.length; i++) {
            int digit = res[i] % 10;
            int carry = res[i] / 10;
            sb.insert(0, digit);
            if (i < res.length - 1) {
                res[i + 1] += carry;
            }
        }
        
        //處理leading zero的問題
        while (sb.length() > 0 && sb.charAt(0) == '0') {
            sb.deleteCharAt(0);
        }
        
        
        return sb.length() == 0 ? "0" : sb.toString();
    }
}
```

---
###Reference
1. http://www.cnblogs.com/springfor/p/3889706.html
2. http://blog.csdn.net/linhuanmars/article/details/20967763