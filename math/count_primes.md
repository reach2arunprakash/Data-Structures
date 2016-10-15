
#Count Primes

[原題網址](https://leetcode.com/problems/count-primes/)

題意：給一個數 n ，找出所有小於 n 的質數。

解題思路：

>[Wiki](https://zh.wikipedia.org/wiki/%E7%B4%A0%E6%95%B0)

>驗證一個數字 n 是否為質數的一種簡單但緩慢的方法為試除法。此一方法會測試 n 是否為任一在2與$$\sqrt{n}$$之間的整數之倍數。比試除法更加有效率的演算法已被發現用來測試較大的數字是否為質數。

暴力法：檢查所有 2與$$\sqrt{n}$$ 的數，令為 i ，則檢查 i是否能被任到小於i 的數整除，若是，則非質數，若非，則是質數，所花的時間為 $$O(N^{2})$$


改進：首先使用一個陣列 isPrime 來判斷該數是否為質數，預先全設為 true ，接著對於每個數 n ，只要該數為質數，則把它的倍數的isPrime設為 false，最後留下來的便全是質數

如 n = 10 則只需檢查 2 到 $$\sqrt{10} = 3.16...$$ 的倍數，即

一開始isPrime的陣列內容如下：

| 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| -- | -- | -- | -- | -- | -- | -- | -- |
| T | T | T | T | T | T | T | T |

i = 2 時，把所有2的倍數，即4，6，8，10，全改為false。 

| 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| -- | -- | -- | -- | -- | -- | -- | -- |
| T | T | F | T | F | T | F | T |

i = 3 時，把所有3的倍數，即6，9全改為false。  

完成後 isPrime的陣列內容如下

| 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| -- | -- | -- | -- | -- | -- | -- | -- |
| T | T | F | T | F | T | F | F |

則其他標示為 T 的元素即為質數，2，3，5，7。

其程式碼如下：

updated on 2016.1.8

```java
public class Solution {
    public int countPrimes(int n) {
        if (n <= 1) {
            return 0;
        }
        
        int[] isPrime = new int[n];
        Arrays.fill(isPrime, 1);
        double sqrtN = Math.sqrt(n);
        
        for (int i = 2; i < sqrtN; i++) {
            if (isPrime[i] == 1) {
                for (int j = i + i; j < n; j = j + i) {
                    isPrime[j] = 0;
                }
            }
            
        }
        
        int count = 0;
        for (int i = 2; i < n; i++) {
            count += isPrime[i];
        }
        
        return count;
    }
}
```