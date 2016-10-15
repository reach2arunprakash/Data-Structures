# Count of Smaller Numbers After Self

[Leetcode](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

題意：

You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].

Example:

Given nums = [5, 2, 6, 1]

To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
Return the array [2, 1, 1, 0].

解題思路：

也可用segment tree來解

使用二分法去找插入位置，回傳的插入位置表示前面有幾個比它小。


程式碼如下：

```java
public class Solution {
    public List<Integer> countSmaller(int[] nums) {
        Integer[] ans = new Integer[nums.length];
        List<Integer> sorted = new ArrayList<Integer>();
        
        for (int i = nums.length - 1; i >= 0; i--) {
            int index = findIndex(sorted, nums[i]);
            ans[i] = index;
            sorted.add(index, nums[i]);
        }
        
        return Arrays.asList(ans);
    }
    
    private int findIndex(List<Integer> sorted, int target) {
        if (sorted.size() == 0) {
            return 0;
        }
        
        int start = 0;
        int end = sorted.size() - 1;
        if (sorted.get(end) < target) {
            return end + 1;
        }
        if (sorted.get(start) > target) {
            return 0;
        }
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (sorted.get(mid) < target) {
                start = mid + 1;
            } else {
                end = mid;
            }
        }
        
        if (sorted.get(start) >= target) {
            return start;
        } else {
            return end;
        }
    }
}
```

---
###Reference
1. 