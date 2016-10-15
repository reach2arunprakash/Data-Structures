# Reverse Bits

[Leetcode](https://leetcode.com/problems/reverse-bits/)

題意：

Reverse bits of a given 32 bits unsigned integer.

For example, given input 43261596 (represented in binary as **00000010100101000001111010011100**), return 964176192 (represented in binary as **00111001011110000010100101000000**).

**Follow up:**

If this function is called many times, how would you optimize it?


解題思路：

利用一個count值從31往1到數，來決定取出來的bit要往右shift幾格，程式碼如下：

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int sum = 0;
        int count = 31;
        while (n != 0) {
            if ((n & 1) == 1) {
                sum += 1 << count;
            }
            count--;
            n >>>= 1;
        }
        return sum;
    }
}
```

---
###Reference
1. https://leetcode.com/discuss/57618/simply-great-performance-java-solution