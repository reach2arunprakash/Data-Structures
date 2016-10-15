# Excel Sheet Column TItle

[Leetcode 1](https://leetcode.com/problems/excel-sheet-column-title/)

[Leetcode 2](https://leetcode.com/problems/excel-sheet-column-number/)

題意：

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:
```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
```

解題思路：

數字轉字串

```java
public class Solution {
    public String convertToTitle(int n) {
        StringBuilder sb = new StringBuilder();
        while (n != 0) {
            int remainder = n % 26;
            n = n / 26;
            if (remainder == 0) {
                sb.append('Z');
                n--;
            } else {
                char c = (char)(remainder - 1 + 'A');
                sb.append(c);
            }
        }
        
        return sb.reverse().toString();
    }
}
```


字串轉數字

```java
public class Solution {
    public int titleToNumber(String s) {
        int sum = 0;
        if (s == null || s.length() == 0) {
            return 0;
        }
        int twentySix = 26;
        int pow = 0;
        for (int i = s.length() - 1; i >= 0; i--) {
            int cur = (int) (s.charAt(i) - 'A' + 1);
            sum += cur * Math.pow(twentySix, pow);
            pow++;
        }
        return sum;
    }
}
```