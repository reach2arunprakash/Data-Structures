# Bulb Switcher

[]()


題意：

There are n bulbs that are initially off. You first turn on all the bulbs. Then, you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the nth round, you only toggle the last bulb. Find how many bulbs are on after n rounds.

Example:

Given n = 3. 
```
At first, the three bulbs are [off, off, off].
After first round, the three bulbs are [on, on, on].
After second round, the three bulbs are [on, off, on].
After third round, the three bulbs are [on, off, off]. 
```
So you should return 1, because there is only one bulb is on.

解題思路：

1. 如果該數為質數，只會被switch兩次，第一輪與他們自己那一輪
2. 其他的數會被開關odd次，要嘛開要嘛關，所以主要我們要找到那些數會被開關odd次以及那些sqrt(n)的數。

>// For prime numbers, they must be off because we can reach them only twice (The first round and their own round).
/* For other numbers, if we can reach them odd times, then they are on; otherwise, they are off. So only 
 those numbers who have square root will be reached odd times and there are sqrt(n) of them because
 for every x > sqrt(n), x*x > n and thus should not be considered as the answer. */
 

```java
public class Solution {
    public int bulbSwitch(int n) {
        int count = 0;
        for (int i = 1; i *i <= n; i++) {
            count++;
        }
        
        return count;
    }
}
```