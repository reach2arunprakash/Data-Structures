# Digit Count

[Lintcode](http://www.lintcode.com/en/problem/digit-counts/)

題意：

Count the number of k's between 0 and n. k can be 0 - 9.


Example

if n=12, k=1 in ```[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]```, we have FIVE 1's (1, 10, 11, 12)

解題思路：

暴力法，從0 到 n 分別丟進去一個helper function來幫助我們算該數含有幾個k，最後回傳，時間複雜度相當高。

程式碼如下：

```java
class Solution {
    /*
     * param k : As description.
     * param n : As description.
     * return: An integer denote the count of digit k in 1..n
     */
    public int digitCounts(int k, int n) {
        
        int count = 0;
        for (int i = 0; i <= n; i++) {
            count += helper(i, k);
        }
        
        return count;
    }
    
    public int helper(int n, int k) {
        int count = 0;
        while (n > 0) {
            if (n % 10 == k) {
                count++;
            }
            n /= 10;
        }
        return count;
    }
};
```
方法二：參考 http://www.hawstein.com/posts/20.4.html 分析一下會發現有如下結論

* 當某一位的數字小於i時，那麼該位出現i的次數為：更高位數字x當前位數
* 當某一位的數字等於i時，那麼該位出現i的次數為：更高位數字x當前位數+低位數字+1
* 當某一位的數字大於i時，那麼該位出現i的次數為：(更高位數字+1)x當前位數

>不是很好理解，之後清楚了再來寫解說

```java
class Solution {
    /*
     * param k : As description.
     * param n : As description.
     * return: An integer denote the count of digit k in 1..n
     */
    public int digitCounts(int k, int n) {
        
        int res = 0;
        int base = 1;
        while ( n / base > 0) {
            int cur = (n / base) % 10;
            int low = n - (n / base) * base;
            int high = n / (base * 10);
            
            if (cur == k) {
                res += high * base + low + 1;
            } else if (cur < k) {
                res += high * base;
            } else {
                res += (high + 1) * base;
            }
            
            base *= 10;
        }
        
        return res;
    }
};


```




---
###Reference
1. http://www.cnblogs.com/EdwardLiu/p/4274497.html 
2. http://www.hawstein.com/posts/20.4.html (內含詳細分析，解說相當精闢)