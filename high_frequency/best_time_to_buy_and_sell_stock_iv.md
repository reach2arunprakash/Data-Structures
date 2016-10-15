#Best Time to Buy and Sell Stock IV

[原題網址](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock-iv/)

題意：為 [Best Time to Buy and Sell Stock III](high_frequency/best_time_to_buy_and_sell_stock_iv.md) 的延伸，由原本的2次交易延伸為k次交易。

解題思路：此為[Maximum Subarray III](array/maximum_subarray_iii.md)的變形，可以透過動態規劃來幫助我們解決這道題。


```java
public int maxProfit(int k, int[] prices) {
        
    if (prices == null || prices.length == 0 || k == 0) {
        return 0;
    }
    
    int len = prices.length;
    
    //由於若k超大的話，則退化為 Best Time to Buy and Sell Stock II，
    if (k >= len / 2) {
        int profit = 0;
        for (int i = 1; i < len; i++) {
            if (prices[i] > prices[i - 1]) {
                profit += prices[i] - prices[i - 1];
            }
        }
        
        return profit;
    }
    
    // mustSell[i][j] 表示前i天作了j次交易，且第j天必須賣的最大獲利為多少。
    // globalBest[i][j] 表示前i天作了j次交易的最大獲利為多少，第i天不一定要賣。
    int[][] mustSell = new int[len][k + 1];
    int[][] globalBest = new int[len][k + 1];
    
    //初始化，由於前1天不管作幾次交易，皆無法賣，所以獲利為 0
    for (int i = 0; i <= k; i++) {
        mustSell[0][i] = globalBest[0][i] = 0;
    }
    
    for (int i = 1; i < len; i++) {
        int gain = prices[i] - prices[i - 1];
        for (int j = 1; j <= k; j++) {
            //globalBest[i - 1][j - 1] + gain，表示在第i - 1天賣掉後，當天馬上又買，接著在第i天賣掉的獲利
            // mustSell[i - 1][j] + gain 表示把原本在第i-1應該要賣掉的股票延到第i天賣。
            mustSell[i][j] = Math.max(globalBest[i - 1][j - 1] + gain, mustSell[i - 1][j] + gain);
            globalBest[i][j] = Math.max(globalBest[i-1][j], mustSell[i][j]);
        }
    }
    
    return globalBest[len - 1][k];
}
```

---
###Reference

1. http://www.jiuzhang.com/solutions/best-time-to-buy-and-sell-stock-iv/