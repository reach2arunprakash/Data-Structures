# Best Time to Buy and Sell Stock

[原題網址](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock/)

題意：給定一個陣列，陣列中每個元素代表著股價，假設只能買賣股票一次，找出最大的獲利值

解題思路：這題其實是 Maximum Subarray 的變形，在這個程式，我們只要利用一個minsum變數來記錄最小的值(逢低買進)，接著如果能創造更高的獲利，則更新獲利值。

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length <= 1) {
        return 0;
    }
    int maxDiff = prices[1] - prices[0];
    int minValue = prices[0];
    for (int i = 1; i < prices.length; i++) {
        if (prices[i] - minValue > maxDiff) {
            maxDiff = prices[i] - minValue;
        }
        if (prices[i] < minValue) {
            minValue = prices[i];
        }
    }
    if (maxDiff < 0) {
        return 0;
    } else {
        return maxDiff;
    }
    
}
```


---
```java
public int maxProfit(int[] prices) {
        
    if (prices == null || prices.length == 0) {
        return 0;
    }
    
    int min = Integer.MAX_VALUE;
    int profit = 0;
    
    for (int i = 0; i < prices.length; i++) {
        profit = Math.max(profit, prices[i] - min);
        min = Math.min(min, prices[i]);
    }
    
    return profit;
}
```
