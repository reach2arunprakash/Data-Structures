# Find the Duplicate Number

[Leetcode](https://leetcode.com/problems/find-the-duplicate-number/)

題意：

Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Note:
1. You must not modify the array (assume the array is read only).
2. You must use only constant, O(1) extra space.
3. Your runtime complexity should be less than O(n2).
4. There is only one duplicate number in the array, but it could be repeated more than once.


解題思路：


O(N^2) 解法，程式碼如下：

```java
public class Solution {
    public int findDuplicate(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] == nums[j]) {
                    return nums[i];
                }
            }
        }
        
        return 0;
    }
}
```

有網友提供了一個類似linked list用快慢指針找cycle的方式，Floyd Cycle Detection，只需要走訪兩遍陣列即可，複雜度為O(N)，其程式碼如下


>使用這個方式的條件是有n+1個數，且值都在1到n之間，否則會有index overflow的問題。


```java
public class Solution {
    public int findDuplicate(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int slow = 0;
        int fast = 0;
        int finder = 0;
        
        while (true) {
            slow = nums[slow];
            fast = nums[nums[fast]];
            if (slow == fast) {
                break;
            }
        }
        
        while (true) {
            slow = nums[slow];
            finder = nums[finder];
            if (slow == finder) {
                return slow;
            }
        }

    }
}
```


---
###Reference
1. https://leetcode.com/discuss/69766/share-my-solution-o-n-time-o-1-space-12-ms
2. https://www.quora.com/How-does-Floyds-cycle-finding-algorithm-work