# A + B Problem

[Lintcode](http://www.lintcode.com/en/problem/a-b-problem/)

題意：

Write a function that add two numbers A and B. You should not use + or any arithmetic operators.

**Note**

There is no need to read data from standard input stream. Both parameters are given in function aplusb, you job is to calculate the sum and return it.

**Challenge**

Of course you can just return a + b to get accepted. But Can you challenge not do it like that?

**Clarification**

Are a and b both 32-bit integers?

Yes.

Can I use bit operation?

Sure you can.

解題思路：

一個bit一個bit 的慢慢判斷，其程式碼如下：

```java
class Solution {
    /*
     * param a: The first integer
     * param b: The second integer
     * return: The sum of a and b
     */
    public int aplusb(int a, int b) {
        int carry = 0;
        int sum = 0;
        for (int i = 0; i < 32; i++) {
            int aBit = (a >> i) & 1;
            int bBit = (b >> i) & 1;
            sum |= (aBit ^ bBit ^ carry) << i;
            if ((aBit == 1 && bBit == 1) || ((aBit == 1 || bBit == 1) && carry == 1)) {
                carry = 1;
            } else {
                carry = 0;
            }
        }
        
        return sum;
    }
};

```
###Reference

1. http://www.cnblogs.com/EdwardLiu/p/4269079.html