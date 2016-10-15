# Summary Ranges

[Leetcode](https://leetcode.com/problems/summary-ranges/)

題意：

Given a sorted integer array without duplicates, return the summary of its ranges.

For example, given [0,1,2,4,5,7], return ["0->2","4->5","7"].


解題思路：

使用兩根指針，一但遇到gap，則插入新的range，接著繼續往下一個開始搜尋，其程式碼如下：

```java
public class Solution {
    public List<String> summaryRanges(int[] nums) {
        List<String> res = new ArrayList<String>();
        
        if (nums == null || nums.length == 0) {
            return res;
        }
        
        int s = 0;
        int e = 0;
        while (e < nums.length) {
            while (e + 1 < nums.length && nums[e] + 1 == nums[e + 1]) {
                e++;
            }
            
            if (s == e) {
                String str = Integer.toString(nums[e]);
                res.add(str);
            } else {
                String str = nums[s] + "->" + nums[e];
                res.add(str);
            }
            e++;
            
            s = e;
        }
        
        return res;
    }
}
```

---
###Reference
1. http://blog.csdn.net/xudli/article/details/46645087