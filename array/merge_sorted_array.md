# Merge Sorted Array I
[原題網址](http://www.lintcode.com/en/problem/merge-sorted-array/)

```java
public void mergeSortedArray(int[] A, int m, int[] B, int n) {
    // 從後面倒過來作
    int posA = m - 1;
    int posB = n - 1;
    int idx = m + n - 1;
    while (posA >= 0 && posB >= 0) {
        if (A[posA] > B[posB]) {
            A[idx--] = A[posA--];
        } else {
            A[idx--] = B[posB--];
        }
    }
    //處理剩下的element
    while (posA >= 0) {
        A[idx--] = A[posA--];
    }
    while (posB >= 0) {
        A[idx--] = B[posB--];
    }
}
```

# Merge Sorted Array II
[原題網址](http://www.lintcode.com/en/problem/merge-sorted-array-ii/)

```java
public ArrayList<Integer> mergeSortedArray(ArrayList<Integer> A, ArrayList<Integer> B) {
    ArrayList<Integer> res = new ArrayList<Integer>();
    if (A == null || A.size() == 0) {
        return B;
    }
    if (B == null || B.size() == 0) {
        return A;
    }

    int pointA = 0;
    int pointB = 0;
    while (pointA < A.size() && pointB < B.size()) {
        if (A.get(pointA) <= B.get(pointB)) {
            res.add(A.get(pointA++));
        } else {
            res.add(B.get(pointB++));
        }
    }
    while (pointA < A.size()) {
        res.add(A.get(pointA++));
    }
    while (pointB < B.size()) {
        res.add(B.get(pointB++));
    }

    return res;
 }
```
