#Longest Continuous Increasing Subsequence

[原題連結](http://www.lintcode.com/en/problem/longest-increasing-continuous-subsequence/)

**題意**：找出一個最長連續遞增/減的子序列。

Example

For ```[5, 4, 2, 1, 3]```, the LICS is ```[5, 4, 2, 1]```, return 4.

For ```[5, 1, 2, 3, 4]```, the LICS is ```[1, 2, 3, 4]```, return 4.

**解題思路**：可以使用二維陣列與動態規劃來作，但是時間與空間各需花 $$O(N^{2})$$ ，我們使用掃法，由左掃到右，利用 count 來紀錄當前的最長連續子陣列的長度，只要遇到遞增即更新 count ，否則重設 count 由於遞增與遞減都算，所以掃了兩次。

```java
public int longestIncreasingContinuousSubsequence(int[] A) {
    
    if (A == null || A.length == 0) {
        return 0;
    }
    
    int count = 0;
    
    for(int i = 0, j = 0; i < A.length;) {
        int temp = 1;
        while (j < A.length - 1 && A[j + 1] >= A[j]) {
            temp++;
            j++;
        }
        count = temp > count ? temp : count;
        j = j + 1;
        i = j;
    } 
}
```

>Time Complexity：$$O(2N) = O(N)$$ ，因為掃了兩次。

>Space Complexity： $$O(1)$$，因只用了幾個變數來紀錄當前的狀況。