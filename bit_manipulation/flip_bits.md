# Flip Bits

[Lintcode](http://www.lintcode.com/en/problem/flip-bits/)

題意：

Determine the number of bits required to flip if you want to convert integer n to integer m.

Given n = 31 (11111), m = 14 (01110), return 2.

Note

Both n and m are 32-bit integers.

解題思路：

一一比較每個位元，若有不同則count++，需要特別注意的就是正負號的問題。若其中只有一個為負數，則 count 需要再加 1 ，為2  的補數關係。

```java
class Solution {
    /**
     *@param a, b: Two integer
     *return: An integer
     */
    public static int bitSwapRequired(int a, int b) {
        int count = 0;
        for (int i = 0; i < 31; i++) {
            int aBit = a >> i & 1;
            int bBit = b >> i & 1;
            if (aBit != bBit) {
                count++;
            }
        }
        
        if (a < 0 && b < 0) {
            return count;
        }
        if (a < 0 || b < 0) {
            count++;
        }
        return count;
    }
};
```