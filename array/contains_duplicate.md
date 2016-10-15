# Contains Duplicate

[Leetcode](https://leetcode.com/problems/contains-duplicate/)

題意：


解題思路：

使用set來作就好了。

```java
public class Solution {
    public boolean containsDuplicate(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return false;
        }
        
        Set<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < nums.length; i++) {
            if (set.contains(nums[i])) {
                return true;
            }
            set.add(nums[i]);
        }
        return false;
    }
}
```