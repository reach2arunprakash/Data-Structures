# Missing Ranges

[Leetcode(https://leetcode.com/problems/missing-ranges/)

題意：

Given a sorted integer array where the range of elements are [lower, upper] inclusive, return its missing ranges.

For example, given [0, 1, 3, 50, 75], lower = 0 and upper = 99, return ["2", "4->49", "51->74", "76->99"].


解題思路：

使用previous與current兩個指針分別紀錄前後的值，若兩者差距大於1的話，則使用get range的function來產生出需要插入的string。

```java
//lower = 5, upper = 10
//nums = 7,
//res ["5-6", "8-10"];

public class Solution {
    public List<String> findMissingRanges(int[] nums, int lower, int upper) {
        List<String> res = new ArrayList<String>();
        int len = nums.length;
        
        //start from lower - 1
        int previous = lower - 1;
        for (int i = 0 ; i <= len; i++) {
            //take care of boundary condition
            int cur = (i == len) ? upper + 1 : nums[i];
            if (cur - previous > 1) {
                res.add(getRange(previous + 1, cur - 1));
            }
            previous = cur;
        }
        return res;
    }
    
    private String getRange(int from, int to) {
        return (from == to) ? String.valueOf(from) : from + "->" + to;
    }
}
```
