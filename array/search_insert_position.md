# Search Insert Position

題意：

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume **NO** duplicates in the array.

Example
```
[1,3,5,6], 5 → 2

[1,3,5,6], 2 → 1

[1,3,5,6], 7 → 4

[1,3,5,6], 0 → 0
```

Challenge
O(log(n)) time

解題思路：

updated 2016.1.17

```java
public class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        
        return left;
    }
}
```

暴力法掃一次，找到比target大的第一個數的位置，花 O(N)，程式碼如下：

```java
public class Solution {
    /** 
     * param A : an integer sorted array
     * param target :  an integer to be inserted
     * return : an integer
     */
    public int searchInsert(int[] A, int target) {
        
        if (A == null || A.length == 0) {
            return 0;
        }
        
        for (int i = 0; i < A.length; i++) {
            if (A[i] >= target) {
                return i;
            }
        }
        return A.length;
    }
}


```

二分法：find the first position >= target


```java
public class Solution {
    /** 
     * param A : an integer sorted array
     * param target :  an integer to be inserted
     * return : an integer
     */
    public int searchInsert(int[] A, int target) {
        
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int start = 0;
        int end = A.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (target == A[mid]) {
                return mid;
            } else if (target < A[mid]) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (A[start] >= target) {
            return start;
        } else if (A[end] >= target) {
            return end;
        } else {
            return end + 1;
        }
    }
}
```

---
###Reference
1. http://www.jiuzhang.com/solutions/search-insert-position/