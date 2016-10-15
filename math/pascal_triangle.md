# Pascal Triangle

[Leetcode](https://leetcode.com/problems/pascals-triangle/)

題意：

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

解題思路：

```java
public class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (numRows == 0) {
            return res;
        }
        List<Integer> pre = new ArrayList<Integer>();
        pre.add(1);
        res.add(pre);
        for (int i = 2; i <= numRows; i++) {
            List<Integer> cur = new ArrayList<Integer>();
            cur.add(1);
            for(int j = 0; j < pre.size() - 1; j++) {
                cur.add(pre.get(j) + pre.get(j + 1));
            }
            cur.add(1);
            pre = cur;
            res.add(cur);
        }
        
        return res;
    }
}
```
---
###Reference
1. http://blog.csdn.net/linhuanmars/article/details/23311527