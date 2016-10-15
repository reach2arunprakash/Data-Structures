# Happy Number

[Leetcode](https://leetcode.com/problems/happy-number/)


題意：

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example: 19 is a happy number

$$1^2 + 9^2 = 82$$

$$8^2 + 2^2 = 68$$

$$6^2 + 8^2 = 100$$


$$1^2 + 0^2 + 0^2 = 1$$


解題思路：

一個數是happy number只以下兩個條件

1. 數有循環，且數中不包含1
2. 或是數最後走到1

```java
public class Solution {
    public boolean isHappy(int n) {
        
        HashSet<Integer> set = new HashSet<Integer>();
        while (n != 1 && !set.contains(n)) {
            set.add(n);
            int sum = 0;
            while (n != 0) {
                sum += Math.pow(n % 10, 2);
                n /= 10;
            }
            n = sum;
        }
        return n == 1;
    }
}
```

---
###Reference
1. http://bookshadow.com/weblog/2015/04/22/leetcode-happy-number/