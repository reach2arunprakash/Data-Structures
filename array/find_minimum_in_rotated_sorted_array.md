# Find Minimum in Rotated Sorted Array

[Lintcode](http://www.lintcode.com/en/problem/find-minimum-in-rotated-sorted-array/)

題意：

Medium Find Minimum in Rotated Sorted Array Show result 


Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.


Given [4, 5, 6, 7, 0, 1, 2] return 0

Note
You may assume no duplicate exists in the array.

解題思路：

因陣列會旋轉，我們需要固定一點 target(在此選陣列中最後一個元素)不斷的拿中間元素與該點比較，會有以下兩個狀況：

1. 若 num[mid] <= target，則 mid在右下，最小值可能在左半邊，移動 end 來繼續找左半邊
2. 若 num[mid] > target，則 mid 在左上，最小值可能在右半邊，移動 start來繼續找右半邊

```java
public int findMin(int[] nums) {
    // write your code here
    if (nums.length == 0) {
        return 0;
    }
    int start = 0;
    int end = nums.length - 1;
    int mid;
    while (start + 1 < end) {
        mid = start + (end - start) / 2;
        //拿end與mid比
        //若mid比end小，代表最小在左半邊，移動end
        //若mid比end大，代表最小在右半邊，移動start
        if (nums[mid] < nums[end]) {
            end = mid;
        } else {
            start = mid;
        }
    }
    //最後剩兩個數，直接比較
    if (nums[start] < nums[end]) {
        return nums[start];
    } else {
        return nums[end];
    }
}
```

---
###Reference
1. http://www.jiuzhang.com/solutions/find-minimum-in-rotated-sorted-array/
