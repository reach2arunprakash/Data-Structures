# Paint Fence

[Leetcode](https://leetcode.com/problems/paint-fence/)

題意：

There is a fence with n posts, each post can be painted with one of the k colors.

You have to paint all the posts such that **no more than two adjacent fence posts have the same color**.

Return the total number of ways you can paint the fence.

Note:
n and k are non-negative integers.


解題思路：

題目要看清楚，一開始理解錯誤，主要是最多兩個相鄰的板子可以不同顏色。

這題可以使用dp來作，主要使用了兩個編數來紀錄，

>LastD：紀錄最後兩塊板子顏色不同的情況數

>LastD = (k - 1) * (lastS + lastD) 因為最後兩塊板子顏色可以不同，因此最新那塊板子有k - 1個顏色可以選擇，加上到數第二塊與第三塊也有不同情況分別為LastS與LastD。

>LastS：紀錄最後兩塊板子顏色相同的情況數，因為最後兩塊相同，所以只要把上一層的LastD assign即可。

```java
public class Solution {
    public int numWays(int n, int k) {
        // 如果板子小於一塊，或是沒半個顏色，則直接返回n * k
        if (n <=  1 || k == 0) {
            return n * k;
        }
        
        int lastS = k; // 最後兩塊板子顏色相同
        int lastD = k * (k - 1); // 最後兩塊板子顏色不同
        for (int i = 2; i < n; i++) {
            int tempD = (k - 1) * (lastS + lastD); // 最後兩塊板子顏色不同，因此最新那塊板子有k - 1個顏色可選
            lastS = lastD; // 最後兩塊板子顏色相同，因此直接取lastD
            lastD = tempD;
        }
        
        return lastS + lastD; // 最後把兩個情況加總即可
    }
}
```

---
###Reference
1. http://blog.csdn.net/pointbreak1/article/details/48779787