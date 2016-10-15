#Bitwise AND of Numbers Range

[Leetcode](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

題意：

Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.

For example, given the range [5, 7], you should return 4.

解題思路：

 網友 [grandyang](http://www.cnblogs.com/grandyang/p/4431646.html) 提供以下兩種思路：
 
 [5, 7]裡共有三個數字，分別寫出它們的二進製為：

101　　110　　111

相與後的結果為100，仔細觀察我們可以得出，最後的數是該數字範圍內所有的數的左邊共同的部分，如果上面那個例子不太明顯，我們再來看一個範圍[26, 30]，它們的二進制如下：

11010　　11011　　11100　　11101　　11110

發現了規律後，我們只要寫程式碼找到左邊公共的部分即可，我們可以從建立一個32位都是1的mask，然後每次向左移一位，比較m和n是否相同，不同再繼續左移一位，直至相同，然後把m和mask相與就是最終結果，程式碼如下：

```java
public class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        int mask = Integer.MAX_VALUE;
        while ((m & mask) != (n & mask)) {
            mask <<= 1;
        }
        
        return (mask == 0) ? 0 : m & mask;
    }
}
```

此題還有另一種解法，不需要用mask，直接平移m和n，每次向右移一位，直到m和n相等，記錄下所有平移的次數i，然後再把m左移i位即為最終結果，程式碼如下：
 
```java
public class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        int count = 0;
        while (m != n) {
            count++;
            m >>= 1;
            n >>= 1;
        }
        return m <<count;
    }
}
```
 
 ---
 ###Reference
 1. http://www.cnblogs.com/grandyang/p/4431646.html