# Candy

[Lintcode](http://www.lintcode.com/en/problem/candy/)

題意：

There are N children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

**Example**

Given ratings = [1, 2], return 3.

Given ratings = [1, 1, 1], return 3.

Given ratings = [1, 2, 2], return 4. ([1,2,1]).

解題思路：

原本自己的解法：不斷的去當前元素的前後元素作比較，若rating比較大則選較大的一邊再加一，但只過了一半的 case，其程式碼如下：

```java
public class Solution {
    /**
     * @param ratings Children's ratings
     * @return the minimum candies you must give
     */
    public int candy(int[] ratings) {
        
        if (ratings == null || ratings.length == 0) {
            return 0;
        }
        
        int minCount = 0;
        int[] count = new int[ratings.length];
        for (int i = 0; i < ratings.length; i++) {
            
            int curCount = 1;
            if ((i - 1) >= 0 && ratings[i] > ratings[i - 1]) {
                curCount = Math.max(curCount, ratings[i - 1] + 1);
            }
            if ((i + 1) < ratings.length && ratings[i] > ratings[i + 1]) {
                curCount = Math.max(curCount, ratings[i + 1] + 1);
            }
            count[i] = curCount;
            minCount += curCount;
        }
        
        return minCount;
    }
}
```

正確解法：應該從左掃一遍，再往右掃一遍，網友 [Fish Lei](http://www.shuatiblog.com/blog/2014/05/31/Candy/) 提供了以下精美的圖

![](http://www.shuatiblog.com/assets/images/candy.png)

程式碼如下：

```java
public class Solution {
    /**
     * @param ratings Children's ratings
     * @return the minimum candies you must give
     */
    public int candy(int[] ratings) {
        
        if (ratings == null || ratings.length <= 1) {
            return 1;
        }
        
        int minCount = 0;
        int len = ratings.length;
        int[] count = new int[ratings.length];
        count[0] = count[len - 1] = 1; // 記得作初始化
        
        // 往右掃，不斷的與前面比，若rating 較大則比前面的糖果數再多一
        for (int i = 1; i < len; i++) {
            if (ratings[i] > ratings[i - 1]) {
                count[i] = Math.max(count[i], count[i - 1] + 1);
            } else {
                count[i] = 1; // 因至少要一顆糖
            }
        }
        
        // 往左掃，不斷的與後面比，若rating 較大則比後面的糖果數再多一
        // 記得與目前的糖果數比較哪個較多
        for (int i = len - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                count[i] = Math.max(count[i], count[i + 1] + 1);
            }
        }
        
        // 掃一次把所有糖果數加起來
        for (int i = 0; i < len; i++) {
            minCount += count[i];
        }
        
        return minCount;
    }
}

```

>Time Complexity：$$O(N)$$

---
###Reference
1. http://www.shuatiblog.com/blog/2014/05/31/Candy/
2. http://zhaohongze.com/wordpress/2013/12/10/leetcode-candy/