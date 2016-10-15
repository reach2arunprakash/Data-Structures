# Move Zeros

[Leetcode](https://leetcode.com/problems/move-zeroes/)

題意：

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.


解題思路：

updated on 2016.1.17

```java
public void moveZeroes(int[] nums) {

    int j = 0;
    for(int i = 0; i < nums.length; i++) {
        if(nums[i] != 0) {
            int temp = nums[j];
            nums[j] = nums[i];
            nums[i] = temp;
            j++;
        }
    }
}
```


```java
public void moveZeroes(int[] nums) {
    int j = 0; // The index of the leftmost zero in nums.
    for(int i = 0; i < nums.length; i++){
        if(nums[i] != 0){
            if(i > j){ // i can only be larger than or equal to j.
                nums[j] = nums[i];
                nums[i] = 0;
            }
            j++;
        }
    }
}
```

使用兩根指針，一個不斷指向陣列中最靠前的0元素，另一根從該指針的後面去找一個元零元素，最後再將兩根指針元素交換即可，其程式碼如下：

```java
public class Solution {
    public void moveZeroes(int[] nums) {
        
        if (nums == null || nums.length == 0) {
            return;
        }
        
        for (int i = 0, j = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                j = i + 1;
                while (j < nums.length && nums[j] == 0) {
                    j++;
                }
                if (j < nums.length) {
                    int temp = nums[i];
                    nums[i] = nums[j];
                    nums[j] = temp;
                }
            }
        }
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/70169/1ms-java-solution