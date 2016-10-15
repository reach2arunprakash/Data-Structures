# Median of Two Sorted Arrays

[原題網址](http://www.lintcode.com/en/problem/median-of-two-sorted-arrays/)

```java
class Solution {

    public double findMedianSortedArrays(int[] A, int[] B) {
        int len = A.length + B.length;
        if (len % 2 == 1) {
            return helper(A, 0, B, 0, len / 2 + 1);
        } else {
            return (helper(A, 0, B, 0, len / 2) + helper(A, 0, B, 0, len / 2 + 1)) / 2.0;
        }
    }

    public int helper(int[] A, int startA, int[] B, int startB, int k) {
        // 因A已拿不出任何東西，直接往B取
        if (startA >= A.length) {
            return B[startB + k - 1];
        }
        if (startB >= B.length) {
            return A[startA + k - 1];
        }
        // 只差一步就到k了，因此取較小的值即可
        if (k == 1) {
            return Integer.min(A[startA], B[startB]);
        }

        // 拿出目前index往後算第k/2的數來比較，取max value是為了不影響後面call function的問題，就是讓它必輸就是了。
        // -1 則是因為取值，index由0開始算。
        int keyA = startA + k / 2 - 1 < A.length ? A[startA + k / 2 - 1] : Integer.MAX_VALUE;
        int keyB = startB + k / 2 - 1 < B.length ? B[startB + k / 2 - 1] : Integer.MAX_VALUE;

        if (keyA < keyB) {
            return helper(A, startA + k / 2, B, startB, k - k / 2);
        } else {
            return helper(A, startA, B, startB + k / 2, k - k / 2);
        }
    }
}
```
