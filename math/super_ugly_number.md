# Super Ugly Number

[Leetcode](https://leetcode.com/problems/super-ugly-number/)

題意：

Write a program to find the nth super ugly number.

Super ugly numbers are positive numbers whose all prime factors are in the gi```ven prime list primes of size k. For example, [1, 2, 4, 7, 8, 13, 14, 16, 19, 26, 28, 32]``` is the sequence of the first 12 super ugly numbers given primes = ``[2, 7, 13, 19]``` of size 4.

Note:
1. 1 is a super ugly number for any given primes.
2. The given numbers in primes are in ascending order.
3. 0 < k ≦ 100, 0 < n ≦ 106, 0 < primes[i] < 1000.


解題思路：

updated on 2016.1.12

維護一個priorityQueue，但queue會爆掉。


```java
public class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        if (n == 0 || primes == null || primes.length == 0) {
            return 0;
        }
        
        PriorityQueue<Integer> q = new PriorityQueue<Integer>();
        q.offer(1);
        for (int i = 0; i < primes.length; i++) {
            q.offer(primes[i]);
        }
        
        while (!q.isEmpty()) {
            int cur = q.poll();
            n--;
            if (n == 0) {
                return cur;
            }

            for (int i = 0; i < primes.length; i++) {
                q.offer(cur * primes[i]);
            }
        }
        
        return 0;
    }
}

```

和原本的ugly number類似

```java
public class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        int len = primes.length;
        int[] index = new int[len];
        int[] res = new int[n];
        res[0] = 1;
        
        for (int i = 1; i < n; i++) {
            int min = Integer.MAX_VALUE;
            for (int j = 0; j < len; j++) {
                min = Math.min(res[index[j]] * primes[j], min);
            }
            res[i] = min;
            for (int j = 0; j < len; j++) {
                if (res[i] % primes[j] == 0) {
                    index[j]++;
                }
            }
        }
        
        return res[n - 1];
    }
}
```

原本的ugly number

```java
public int nthUglyNumber(int n){
    int i2=0, i3=0, i5=0;
    int[] k = new int[n];
    k[0] = 1;
    for (int i=1; i<n; i++) {
        k[i] = Math.min(k[i2]*2, Math.min(k[i3]*3, k[i5]*5));
        if (k[i]%2 == 0) i2++;
        if (k[i]%3 == 0) i3++;
        if (k[i]%5 == 0) i5++;
    }

    return k[n-1];
}
```

---
###Reference
1. https://leetcode.com/discuss/72835/108ms-easy-to-understand-java-solution