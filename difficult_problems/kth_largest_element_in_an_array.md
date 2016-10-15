# Kth Largest Element in an Array

[Leetcode](https://leetcode.com/problems/kth-largest-element-in-an-array/)

題意：

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,

Given ```[3,2,1,5,6,4]``` and k = 2, return 5.

Note: 

You may assume k is always valid, 1 ≦ k ≦ array's length.

解題思路：

使用 quick select來達到 O(N)的時間複雜度，不斷的用pivot把陣列切兩半，其程式碼如下：

```java
public class Solution {
    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        if (k > len) {
            return 0;
        }
        return quickSelect(nums, 0, len - 1, len - k);
    }
    
    private int quickSelect(int[] nums, int left, int right, int k) {
        int pivot = right;
        int num = nums[pivot];
        int low = left;
        int high = right;
        
        while (low < high) {
            //從low開始找到一個比pivot大的數
            while (low < high && nums[low] < num) {
                low++;
            }
            // 從high開始找到一個比pivot小的數
            // 設>=表示忽略pivot
            while (low < high && nums[high] >= num) {
                high--;
            }
            swap(nums, low, high);
        }
        // 交換pivot與目前high指向的位置
        // 因pivot選最後一位，因此必須選一個比pivot大的數來作交換
        // 此時low指針已high指針已重疊，即該數大於pivot，直接作交換
        swap(nums, low, pivot);
        
        if (low == k) {
            return nums[low];
        }
        if (low > k) {
            return quickSelect(nums, left, low - 1, k);
        } else {
            //因為陣列的index都還在，所以直接丟k進去即可
            return quickSelect(nums, low + 1, right, k);
        }
        
    }
    
    private void swap(int[] nums, int idxA, int idxB) {
        int temp = nums[idxA];
        nums[idxA] = nums[idxB];
        nums[idxB] = temp;
    }
}
```

updated 2015.12.1

```java
public class Solution {
    public int findKthLargest(int[] nums, int k) {
        if (nums == null || k == 0) {
            return 0;
        }
        return quickSelect(nums, 0, nums.length - 1, k);
    }
    
    private int quickSelect(int[] nums, int start, int end, int k) {
        int pivot = end;
        int left = start;
        int right = end - 1;
        
        while (left <= right) {
            if (nums[left] > nums[pivot]) {
                swap(nums, left, right);
                right--;
            } else {
                left++;
            }
        }
        
        swap(nums, left, pivot);
        int rank = nums.length - left;
        if (rank == k) {
            return nums[left];
        }
        if (rank > k) {
            return quickSelect(nums, left + 1, end, k);
        } else {
            return quickSelect(nums, start, left - 1, k);
        }
    }
    
    private void swap(int[] nums, int a, int b) {
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/59628/java-concise-quickselect