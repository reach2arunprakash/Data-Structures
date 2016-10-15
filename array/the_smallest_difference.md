#The Smallest Difference

[原題連結](http://www.lintcode.com/en/problem/the-smallest-difference/)

題意：給兩個陣列 A 與 B 在兩個陣列中各選一個元素相減，找出最小的差。

解題思路：可用暴力法一一比對，花 $$O(N^{2})$$ 時間複雜度，但可以透過排序兩個陣列，利用兩個指針來幫助我們計算出最小差，程式碼如下：

```java
public int smallestDifference(int[] A, int[] B) {
    
    Arrays.sort(A);
    Arrays.sort(B);
    
    int i = 0;
    int j = 0;
    int minDiff = Integer.MAX_VALUE;
    while ( i < A.length && j < B.length) {
        int curDiff = Math.abs(A[i] - B[j]);
        minDiff = (curDiff < minDiff) ? curDiff : minDiff;
        if (A[i] > B[j]) {
            j++;
        } else {
            i++;
        }
    }
    
    return minDiff;
}
```
>Time Complexity：$$O(NlogN)$$