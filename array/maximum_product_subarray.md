#Maximum Product Subarray

[原題連結](http://www.lintcode.com/en/problem/maximum-product-subarray/)

題意：給一個陣列，找出一個子陣列其乘積為最大。

解題思路：也是與 [Maximum Subarray](array/maximum_subarray.md)，類似不斷的維護全局的最大值，與 local 的最大與最小值，來求得最終結果。但這裡需要特別注意的地方是，原本很小的負數，可能因為乘了另一個負數，變成一個很大的正數，因此也要紀錄local min的部份，程式碼如下：

```java
public int maxProduct(int[] nums) {
    
    if (nums == null || nums.length == 0) {
        return 1;
    }
    
    int global = nums[0];
    int localMax = nums[0];
    int localMin = nums[0];
    for (int i = 1; i < nums.length; i++) {
        
        // localMax會被改變，因此得先存下來。
        int localMaxCopy = localMax;

        // 作比較時，也需與當前陣列元素作比較，來決定是否放棄前面的乘積
        localMax = Math.max(Math.max(localMax * nums[i], localMin * nums[i]), nums[i]);
        localMin = Math.min(Math.min(localMin *nums[i], localMaxCopy * nums[i]), nums[i]);
        global = Math.max(global, localMax);
    }
    
    return global;
}
```

>Time Complexity： $$O(N)$$，因只需遍歷陣列一次。

---
###Reference
1. http://blog.csdn.net/linhuanmars/article/details/21314059