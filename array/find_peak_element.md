# Find Peak Element

[原題網址](http://www.lintcode.com/en/problem/find-peak-element/)

```java
public int findPeak(int[] A) {
    // write your code here

    int start = 1;
    int end = A.length - 2;
    int mid;
    // 用start + 1是代表start 與end中間至少要有一個mid
    while (start + 1 < end) {
        mid = start + (end - start) / 2;
        //拿mid與兩邊比小，若mid比左邊小，代表左邊有峰值
        //若mid比右邊小，代表右邊有峰值
        //若else，代表mid比左邊大，且比右邊大，則mid本身可能是峰值。
        if (A[mid] < A[mid - 1]) {
            end = mid;
        } else if (A[mid] < A[mid + 1]) {
            start = mid;
        } else {
            end = mid;
        }
    }
    //最後要double check一下誰是peak
    if (A[start] > A[end]) {
        return start;
    } else {
        return end;
    }
}
```
