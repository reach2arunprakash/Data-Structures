# Different Ways to Add Parentheses

[Leetcode](https://leetcode.com/problems/different-ways-to-add-parentheses/)


題意：

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are +, - and *.


Example 1
Input: ```"2-1-1"```
```
((2-1)-1) = 0
(2-(1-1)) = 2
Output: ```[0, 2]```
```

Example 2
Input: ```"2*3-4*5"```
```
(2*(3-(4*5))) = -34
((2*3)-(4*5)) = -14
((2*(3-4))*5) = -10
(2*((3-4)*5)) = -10
(((2*3)-4)*5) = 10
```
Output: ```[-34, -14, -10, -10, 10]```

解題思路：

使用遞迴去作，一遇到operator就把字串切成兩半分別遞迴，之後再根字operator把所有回傳的結果組合起來，再往上一層傳。

```java
public class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> res = new ArrayList<Integer>();
        for (int i = 0; i < input.length(); i++) {
            char c = input.charAt(i);
            if (c == '+' || c == '-' || c == '*') {
                String strA = input.substring(0, i);
                String strB = input.substring(i + 1);
                
                List<Integer> partA = diffWaysToCompute(strA);
                List<Integer> partB = diffWaysToCompute(strB);
                
                for (int a : partA) {
                    for (int b : partB) {
                        if (c == '+') {
                            res.add(a + b);
                        } else if (c == '-') {
                            res.add(a - b);
                        } else if (c == '*') {
                            res.add(a * b);
                        }
                    }
                }
            }
        }
        
        if (res.size() == 0) res.add(Integer.valueOf(input));
        return res;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/60626/share-a-clean-and-short-java-solution