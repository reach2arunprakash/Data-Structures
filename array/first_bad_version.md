# First Bad Version
[原題網址](http://www.lintcode.com/en/problem/first-bad-version/)

>解法：其實這題就是二分搜尋的變形

```java
public int findFirstBadVersion(int n) {
    if ( n == 1 ) {
        if (VersionControl.isBadVersion(1))
            return 1;
        else
            return 0;
    }

    int start = 1;
    int end = n;
    int mid;
    boolean midIsBad;
    while (start + 1 < end) {
        mid = start + (end - start) / 2;
        midIsBad = VersionControl.isBadVersion(mid);
        //若mid是bad，則第一個bad在前半段
        //否則bad會發生在後半段
        if (midIsBad) {
            end = mid;
        } else {
            start = mid;
        }
    }
    if (VersionControl.isBadVersion(start)) {
        return start;
    }
    if (VersionControl.isBadVersion(end)) {
        return end;
    }
    return 0;
}

```
