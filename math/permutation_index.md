# Permutation Index

[Lintcode](http://www.lintcode.com/en/problem/permutation-index/)

題意：

Given a permutation which contains no repeated number, find its index in all the permutations of these numbers, which are ordered in lexicographical order. The index begins at 1.

Example

Given [1,2,4], return 1.

解題思路：

暴力法：把所有的permutation產生出來，直接讀第 index 個，時間複雜O(N!)

解法二：

引用網友XY [bubuko](http://www.bubuko.com/infodetail-1144348.html) 的思路解說：

"1.對於四位數：4213 = 4*100+2*100+1*10+3

2.4個數的排列有4！種。當我們知道第一位數的時候，還有3！種方式，當知道第二位數時候還有2！種方式，當知道第三位數的時候還有1！種方式，前面三位數都確定的時候，最後一位也確定了。<這裡是按照高位到地位的順序>

3.對4個數的排列，各位的權值為：3！，2！，1！，0！。第一位之後的數小於第一位的個數是x，第二位之後的數小於第二位的個數是y，第三位之後的數小於第三的個數是z，第四位之後的數小於第四位的個數是w，則abcd排列所在的序列號:index = x*3！+y*2!+z*1!, 0! /= 0

在數的排列中，小數在前面，大數在後面，所以考慮該位數之後的數小於該為的數的個數，這裡我自己理解的也不是很透，就這樣。

4.例如 4213；x= 3，y = 1，z=0，index = 18+2=20

123；x = 0，y=0，index = 0

321；x= 2，y=1，index = 2 x 2！+1 x 1！ = 5

這裡的下標是從0開始的。"

>就是由後往前作，每固定一個數，去算後面比該數小的元素個數，index再加上元素個數乘上factor即可。

程式碼如下
```java
public class Solution {
    /**
     * @param A an integer array
     * @return a long integer
     */
    public long permutationIndex(int[] A) {
        
        if (A == null || A.length == 0) {
            return 0;
        }
        
        long index = 1;
        long factor = 1;
        for (int i = A.length - 2; i>= 0; i--) {
            int rank = 0;
            for (int j = i + 1; j < A.length; j++) {
                if (A[i] > A[j]) {
                    rank++;
                }
            }
            index += rank * factor;
            factor *= (A.length - i);
        }
        
        return index;
    }
}
```

---
###Reference
1. http://algorithm.yuanbin.me/zh-cn/exhaustive_search/permutation_index.html
2. http://www.bubuko.com/infodetail-1144348.html