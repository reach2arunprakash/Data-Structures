#Best Time to Buy and Sell Stock with Cooldown 


題意：


解題思路：


updated on 2016.1.1

以下由網友 [書影](http://bookshadow.com/weblog/2015/11/24/leetcode-best-time-to-buy-and-sell-stock-with-cooldown/)提供精美的說明

引入輔助數組sells和buys
```
sells[i]表示在第i天賣出股票所能獲得的最大累積收益
buys[i]表示在第i天買入股票所能獲得的最大累積收益
```
初始化令sells[0] = 0，buys[0] = -prices[0]

記第i天與第i-1天的價格差：delta = price[i] - price[i - 1]

狀態轉移方程為：
```
sells[i] = max(buys[i - 1] + prices[i], sells[i - 1] + delta) 
buys[i] = max(sells[i - 2] - prices[i], buys[i - 1] - delta)
```
上述方程的含義為：
```
第i天賣出的最大累積收益 = max(第i-1天買入~第i天賣出的最大累積收益, 第i-1天賣出後反悔~改為第i天賣出的最大累積收益)
第i天買入的最大累積收益 = max(第i-2天賣出~第i天買入的最大累積收益, 第i-1天買入後反悔~改為第i天買入的最大累積收益)
```
而實際上：
```
第i-1天賣出後反悔，改為第i天賣出 等價於 第i-1天持有股票，第i天再賣出
第i-1天買入後反悔，改為第i天買入 等價於 第i-1天沒有股票，第i天再買入
```

```java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        
        int len = prices.length;
        int[] buys = new int[len];
        int[] sells = new int[len];
        int max = Integer.MIN_VALUE;
        buys[0] = -prices[0];
        sells[0] = 0;
        
        for (int i = 1; i < prices.length; i++) {
            int delta = prices[i] - prices[i - 1];
            
            sells[i] = Math.max(buys[i - 1] + prices[i], sells[i - 1] + delta);
            
            int temp = (i > 1) ? sells[i - 2] - prices[i] : 0;
            buys[i] = Math.max(buys[i - 1] - delta, temp);
            
            max = Math.max(sells[i], max);
        }
        
        return max;
        
        
    }
}
```

或

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        size = len(prices)
        if size < 2:
            return 0
        buys = [None] * size
        sells = [None] * size
        sells[0], sells[1] = 0, max(0, prices[1] - prices[0])
        buys[0], buys[1] = -prices[0], max(-prices[0], -prices[1])
        for x in range(2, size):
            sells[x] = max(sells[x - 1], buys[x - 1] + prices[x])
            buys[x] = max(buys[x - 1], sells[x - 2] - prices[x])
        return sells[-1]
```

**1. Define States**

To represent the decision at index i:

>buy[i]: Max profit till index i. The series of transaction is ending with a buy.

>sell[i]: Max profit till index i. The series of transaction is ending with a sell.
To clarify:

Till index i, the buy / sell action must happen and must be the last action. It may not happen at index i. It may happen at i - 1, i - 2, ... 0.
In the end n - 1, return sell[n - 1]. Apparently we cannot finally end up with a buy. In that case, we would rather take a rest at n - 1.
For special case no transaction at all, classify it as sell[i], so that in the end, we can still return sell[n - 1]. Thanks @alex153 @kennethliaoke @anshu2.

**2. Define Recursion**

buy[i]: To make a decision whether to buy at i, we either take a rest, by just using the old decision at i - 1, or sell at/before i - 2, then buy at i, We cannot sell at i - 1, then buy at i, because of cooldown.
sell[i]: To make a decision whether to sell at i, we either take a rest, by just using the old decision at i - 1, or buy at/before i - 1, then sell at i.
So we get the following formula:
```
buy[i] = Math.max(buy[i - 1], sell[i - 2] - prices[i]);   
sell[i] = Math.max(sell[i - 1], buy[i - 1] + prices[i]);
```
3. Optimize to O(1) Space

DP solution only depending on i - 1 and i - 2 can be optimized using O(1) space.

Let b2, b1, b0 represent buy[i - 2], buy[i - 1], buy[i]
Let s2, s1, s0 represent sell[i - 2], sell[i - 1], sell[i]
Then arrays turn into Fibonacci like recursion:

>b0 = Math.max(b1, s2 - prices[i]);

>s0 = Math.max(s1, b1 + prices[i]);

4. Write Code in 5 Minutes

First we define the initial states at i = 0:

We can buy. The max profit at i = 0 ending with a buy is -prices[0].
We cannot sell. The max profit at i = 0 ending with a sell is 0.
Here is my solution. Hope it helps!

```java
public int maxProfit(int[] prices) {
    if(prices == null || prices.length <= 1) return 0;

    int b0 = -prices[0], b1 = b0;
    int s0 = 0, s1 = 0, s2 = 0;

    for(int i = 1; i < prices.length; i++) {
        b0 = Math.max(b1, s2 - prices[i]);
        s0 = Math.max(s1, b1 + prices[i]);
        b1 = b0; s2 = s1; s1 = s0; 
    }
    return s0;
}
```



---
###Reference
1. https://leetcode.com/discuss/71391/easiest-java-solution-with-explanations