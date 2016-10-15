# Pascal Triangle II

[Leetcode](https://leetcode.com/problems/pascals-triangle-ii/)

題意：

Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return [1,3,3,1].

Note:

Could you optimize your algorithm to use only O(k) extra space?


解題思路：

每次從最後面加1，再由後往前更新即可。

```java
public class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> res = new ArrayList<Integer>();
        
        for (int i = 0; i <= rowIndex; i++) {
            res.add(1);
            
            for (int j = i-1; j > 0; j--) {
                res.set(j, res.get(j-1) + res.get(j));
            }
        }
        return res;
    }
}
```