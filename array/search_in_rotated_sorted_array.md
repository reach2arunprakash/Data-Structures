# Search in Rotated Sorted Array

[Lintcode](http://www.lintcode.com/en/problem/search-in-rotated-sorted-array/)
題意：

解題思路：

暴力法：掃一次陣列即可，花 O(N)。

二分法：花 O(logN)

類似[Find Minimum in Rotated Sorted Array]() 的作法，但要分兩個情況(1，2)來處理

1：start < mid

2：start >= mid

接著再判斷 target是在哪個區間
若是第一個情況的話，則比較target是否在 start 與  mid之間。
1. 是的話，則把 end 移到 mid
2. 否的話，把 start 移到 mid

若是第二個情況的話，則比較target是否在mid與 end之間。
1. 是的話，則把start 移到 mid
2. 否的話，把 end 移到 mid

每一輪作完仍是一個縮減版的滾動陣列，便繼續往下作。

由於這模板是把搜尋範圍縮短到兩個值，最後記得跟 target 比較再決定輸出，其程式碼如下：


```java
public class Solution {
    /** 
     *@param A : an integer rotated sorted array
     *@param target :  an integer to be searched
     *return : an integer
     */
    public int search(int[] A, int target) {
        
        if (A == null || A.length == 0) {
            return -1;
        }
        
        int start = 0; 
        int end = A.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (target == A[mid]) {
                return mid;
            }
            
            if (A[mid] < A[end]) {
                if (target >= A[mid] && target <= A[end]) {
                    start = mid;
                } else {
                    end = mid;
                }
            } else {
                if (target >= A[start] && target <= A[mid]) {
                    end = mid;
                } else {
                    start = mid;
                }
            }
        }
        
        if (A[start] == target) {
            return start;
        } else if (A[end] == target) {
            return end;
        } else {
            return -1;
        }
    }
}


```
---
###Reference
1. http://www.jiuzhang.com/solutions/search-in-rotated-sorted-array/