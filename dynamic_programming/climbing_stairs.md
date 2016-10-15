# Climbing Stairs

[Leetcode](https://leetcode.com/problems/climbing-stairs/)

題意：

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

解題思路：

其實就是fibonacci number，可用遞迴作法，也可以用動規來幫簡化複雜度

>res[i] = res[i - 1] + res[i - 2]

其程式碼如下：

```java
public class Solution {
    public int climbStairs(int n) {
        if (n <= 2) {
            return n;
        }
        
        int[] result = new int[n];
        result[0] = 1;
        result[1] = 2;
        
        for (int i = 2; i < n; i++) {
            result[i] = result[i-1] + result[i-2];
        }
        return result[n-1];
    }
}
```