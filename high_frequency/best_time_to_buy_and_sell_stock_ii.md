# Best Time to Buy and Sell Stock II

[原題連結](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock-ii/)

題意：與 [Best Time to Buy and Sell Stock](high_frequency/best_time_to_buy_and_sell_stock.md) 類似，但是你可以作無限多次的買賣，唯一的限制是在賣之前你必須先買。

解題思路：因為你可以作無限多次的買賣，因此只要不斷檢查今天的股價是否比前一天高，若是，則購買，否則放棄，程式碼如下。

```java
public int maxProfit(int[] prices) {
        
    int profit = 0;
    
    for (int i = 1; i < prices.length; i++) {
        if (prices[i - 1] < prices[i]) {
            profit += prices[i] - prices[i - 1];
        }
    }
    
    return profit;
}
```

>Time Complexity：$$O(N)$$，因只需遍歷陣列一次。