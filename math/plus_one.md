# Plus One

[Leetcode](https://leetcode.com/problems/plus-one/)

題意：

Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.


解題思路：

參考以下comments，程式碼如下：

```java
public class Solution {
    public int[] plusOne(int[] digits) {
        int len = digits.length;
        
        for(int i = len - 1; i >= 0; i--) {
            if (digits[i] < 9) {
                // 因為小於9的話表示後面的數不會再進位
                // 直接返回即可
                digits[i]++;
                return digits;
            }
            // 到這裡表示 digits[i] == 9加一會變10進位
            digits[i] = 0;
            
        }
        
        // 到這裡表示每一位都是9，會進位成長度為len + 1的數
        // 建新的len + 1陣列，並且把most significant bit設為1
        int[] newNumber = new int[len + 1];
        newNumber[0] = 1;
        return newNumber;
    }
}
```

---
###Reference
1. https://leetcode.com/discuss/58149/my-simple-java-solution