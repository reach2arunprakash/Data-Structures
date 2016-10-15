# H-Index II

[Leetcode](https://leetcode.com/problems/h-index-ii/)


題意：

Follow up for H-Index: What if the citations array is sorted in ascending order? Could you optimize your algorithm?

**Hint:**

Expected runtime complexity is in O(log n) and the input is sorted.


解題思路：

使用二分搜尋法，程式碼如下：

```java
public class Solution {
    public int hIndex(int[] citations) {
        if (citations == null || citations.length == 0) {
            return 0;
        }
        
        int len = citations.length;
        int start = 0;
        int end = citations.length - 1;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (citations[mid] >= len - mid) {
                end = mid - 1;
            } else {
                start = mid + 1;
            }
        }
        
        return len - start;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/56279/concise-standard-binary-search-solution-detailed-explanation