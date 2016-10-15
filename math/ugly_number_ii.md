# Ugly Number II

[]()

題意：

為 ugly number的 follow up，要找出第k個ugly number。

解題思路：

參考網友[nisxiya](http://blog.csdn.net/nisxiya/article/details/46767595)的解說：

使用 primes 數組來記錄前 k 個ugly_number。在生成 primes 數組時，新增入的元素，肯定是該 primes 數組中前面某些元素乘以3，或是乘以5，或是乘以7的結果，且是最小的。依據此思想，可以構建如下的代碼

```java
class Solution {
    /**
     * @param k: The number k.
     * @return: The kth prime number as description.
     */
    public long kthPrimeNumber(int k) {
        
        if (k < 0) {
            return -1;
        }
        
        long[] res = new long[k + 1];
        int i3 = 0;
        int i5 = 0;
        int i7 = 0;
        res[0] = 1;
        
        for (int i = 1; i <= k; i++) {
            long nextNumber = Math.min(res[i3] * 3, Math.min(res[i5] * 5, res[i7] * 7));
            
            if (res[i3] * 3 == nextNumber) {
                i3++;
            } 
            if (res[i5] * 5 == nextNumber) {
                i5++;
            } 
            if (res[i7] * 7 == nextNumber) {
                i7++;
            }
            res[i] = nextNumber;
            
        }
        
        return res[k];
    }
};
```

updated on 2016.1.19

```java
public int nthUglyNumber(int n) {
    if(n==1) return 1;
    PriorityQueue<Long> q = new PriorityQueue();
    q.add(1l);

    for(long i=1; i<n; i++) {
        long tmp = q.poll();
        while(!q.isEmpty() && q.peek()==tmp) tmp = q.poll();

        q.add(tmp*2);
        q.add(tmp*3);
        q.add(tmp*5);
    }
    return q.poll().intValue();
}
```

>Time Complexity：O(k)，Space Complexity：O(K)