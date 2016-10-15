# Update Bits

[Lintcode](http://www.lintcode.com/en/problem/update-bits/)

題意：

Given two 32-bit numbers, N and M, and two bit positions, i and j. Write a method to set all bits between i and j in N equal to M (e g , M becomes a substring of N located at i and starting at j)

Example

Given N=(10000000000)2, M=(10101)2, i=2, j=6

return N=(10001010100)2



解題思路：

先生出mask，再原本要改那段改為0，之後再位移 m 之後再合併原本的，但此法會超時，假始n或m很大時。

```java
class Solution {
    /**
     *@param n, m: Two integer
     *@param i, j: Two bit positions
     *return: An integer
     */
    public int updateBits(int n, int m, int i, int j) {
        //Create mask: xxx00000xxx
        long rightMask = ~0 >> i;
        rightMask = ~(rightMask << i);// 00000xxx
        long leftMask = ~0 >> (j + 1);
        leftMask = leftMask << (j + 1);//xxxxx00000000
        long mask = leftMask | rightMask;//xxx00000xxx
        n = (int) (n & mask);
        n = (int) (n | (m << i));
        return n;
    }
}
```

另外九章給了一個很elegant 的解法，

用以下步驟製作mask，假設我們要 i = 2, j = 6

init : 00000000

1. 1 << (j + 1) 把1往左推了j + 1 位  10000000
2. (1 << i) 把1往前推了i位           00000100 
3. 兩者相減 為                       01111100
4. 再作反轉                          10000011

接著再將原本的字串與mask作and動作，把中間那段清空，

再把m字串往左推i次，再把兩個字串or 起來即可。

>需要特別注意的就是當j非常大的時候，mask即是只要往左移i 格再減一即可。


程式碼如下：

```java
class Solution {
    /**
     *@param n, m: Two integer
     *@param i, j: Two bit positions
     *return: An integer
     */
    public int updateBits(int n, int m, int i, int j) {
        int mask;
        
        if (j < 31) {
            mask = ~(((1 << (j + 1)) -(1 << i)));
        } else {
            mask = ( 1 << i) - 1;
        }
        
        
        return (n & mask) + m << i;
    }
}

```