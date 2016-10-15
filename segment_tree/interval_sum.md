#Interval Sum I
[原題網址](http://www.lintcode.com/en/problem/interval-sum/)

求某區間內的總和，因陣列不會改變，因此可以另外建立一個 sum 的陣列，sum[i] 代表由 A[0] 加到A[i]的總和。

若區間為 (x , y)，等同求 sum[y] - sum[x-1]，程式碼如下。

```java
public ArrayList<Long> intervalSum(int[] A, 
                                   ArrayList<Interval> queries) {
    
    ArrayList<Long> res = new ArrayList<Long>();
    
    if (A == null || A.length == 0 || queries == null || queries.size() == 0) {
        return res;
    }
    
    long[] sum = new long[A.length];
    sum[0] = A[0];
    
    for (int i = 1; i < A.length; i++) {
        sum[i] = A[i] + sum[i-1];
    }
    
    for (int i = 0; i < queries.size(); i++) {
        Interval cur = queries.get(i);
        int start = cur.start;
        int end = cur.end;
        long curRes;
        
        if (start >= 0 && start < A.length && end >= 0 && end < A.length) {
            if (start == 0) {
                curRes = sum[end];
            } else {
                curRes = sum[end] - sum[start - 1];
            }
            res.add(curRes);
        }    
    }
    
    return res;
}
```
