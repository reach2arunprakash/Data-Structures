# Wiggle Sort

[Leetcode](https://leetcode.com/problems/wiggle-sort/)

題意：

Given an unsorted array nums, reorder it in-place such that nums[0] <= nums[1] >= nums[2] <= nums[3]....

For example, given nums = [3, 5, 2, 1, 6, 4], one possible answer is [1, 6, 2, 5, 3, 4].


解題思路：

updated on 2016.1.9

O(n) 解法：

直接不斷的與前一個作比較，並依index的奇偶性值來判斷

```java
public class Solution {
    public void wiggleSort(int[] nums) {
        if (nums == null || nums.length < 2) {
            return;
        }
        
        for (int i = 1; i < nums.length; i++) {
            if ((i % 2 == 0 && nums[i - 1] < nums[i]) || (i % 2 == 1 && nums[i - 1] > nums[i])) {
                int temp = nums[i - 1];
                nums[i - 1] = nums[i];
                nums[i] = temp;
            }
        }
    }
}
```

O(nlogn)解法：

先排序花o(nlogn)，接著遇到奇數位時，把它和相鄰的交換即可。

```java
public class Solution {
    public void wiggleSort(int[] nums) {
        Arrays.sort(nums);

        for(int i = 0; i < nums.length - 1; i++) {
            if(i % 2 == 1) {
                int temp = nums[i];
                nums[i] = nums[i + 1];
                nums[i + 1] = temp;
            }
        }
    }
}
```

i等於偶數時，找最小的來換，奇數時，找最大的來換，花O(N^2)。

```java
public class Solution {
    public void wiggleSort(int[] nums) {
        if (nums == null || nums.length < 2) {
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            int candidate = i;
            for (int j = i; j < nums.length; j++) {
                if (i % 2 == 0) {
                    if (nums[j] < nums[candidate]) {
                        candidate = j;
                    }
                } else {
                    if (nums[j] > nums[candidate]) {
                        candidate = j;
                    }
                }
            }
            int temp = nums[i];
            nums[i] = nums[candidate];
            nums[candidate] = temp;
        }
    }
}
```