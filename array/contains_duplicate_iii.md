# Contains Duplicate III

[]()

題意：

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the difference between nums[i] and nums[j] is at most t and the difference between i and j is at most k.


解題思路：

>The ```ceilingKey(K key)``` method is used to return the least key greater than or equal to the given key, or null if there is no such key.

由[網友](https://codesolutiony.wordpress.com/2015/06/01/leetcode-contains-duplicate-iii/) 提供以下思路：

> 用BST來維護前k個數，因此對於每個數，刪除第i-k-1個數是lgn，得到ceiling和lower key分別是lgn，最後將當前數加入BST是lgn。

```java
public class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if (nums == null) {
            return false;
        }
        
        TreeMap<Long, Integer> map = new TreeMap<Long, Integer>();
        
        for (int i = 0; i < nums.length; i++) {
            if (i > k) {
                map.remove((long)nums[i - k - 1]);
            }
            long val = (long)nums[i];
            Long greater = map.ceilingKey(val);
            if (greater != null && greater - val <= t) {
                return true;
            }
            Long smaller = map.lowerKey(val);
            if (smaller != null && val - smaller <= t) {
                return true;
            }
            map.put(val, i);
        }
        return false;
    }
}
```

---
###Reference
1. https://codesolutiony.wordpress.com/2015/06/01/leetcode-contains-duplicate-iii/