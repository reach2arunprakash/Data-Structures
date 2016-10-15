# Power of Three

[Leetcode](https://leetcode.com/problems/power-of-three/)

題意：

Given an integer, write a function to determine if it is a power of three.

Follow up:
Could you do it without using any loop / recursion?


解題思路：

其實這是個數學題，如果n為power of 3的話，n可以表示為$$ n = 3^x$$，兩邊取log後變成 $$log(n) = x *log(3)$$移項後變成$$ x = log(n) / log(3)$$，接下來我們只要再把x當成power去比較 $$n == 3^n$$與否即可。


```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return n == 0 ? false : n == Math.pow(3, Math.round(Math.log(n) / Math.log(3)));
    }
}
```