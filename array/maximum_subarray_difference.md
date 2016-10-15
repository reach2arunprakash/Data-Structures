#Maximum Subarray Difference

[原題連結](http://www.lintcode.com/en/problem/maximum-subarray-difference/)

題意：給定一個陣列，找出兩個子陣列的差為最大。

解題思路：
可以用四個輔助陣列分別存每個 index 左側的最大連續和，左側的做小連續和，右側的最大連續和，右側的最小連續和，$$O(n)$$搞定。
然後對每個index計算```abs(lGlobalMin[i] - rGlobalMax[i+1])```和```abs(lGlobalMax[i] - rGlobalMin[i+1])```，找出最大值即可。
整體時間$$O(n)$$，空間$$O(n)$$

```java
public int maxDiffSubArrays(ArrayList<Integer> nums) {
    
    if (nums == null || nums.size() == 0) {
        return 0;
    }
    
    int len = nums.size();
    int lLocalMax = nums.get(0);
    int lLocalMin = nums.get(0);
    int[] lGlobalMax = new int[len];
    int[] lGlobalMin = new int[len];
    lGlobalMax[0] = lLocalMax;
    lGlobalMin[0] = lLocalMin;
    
    for (int i = 1; i < len; i++) {
        lLocalMax = Math.max(nums.get(i), lLocalMax + nums.get(i));
        lGlobalMax[i] = Math.max(lGlobalMax[i - 1], lLocalMax);
        lLocalMin = Math.min(nums.get(i), lLocalMin + nums.get(i));
        lGlobalMin[i] = Math.min(lGlobalMin[i - 1], lLocalMin); 
    }
    
    int rLocalMax = nums.get(len - 1);
    int rLocalMin = nums.get(len - 1);
    int[] rGlobalMax = new int[len];
    int[] rGlobalMin = new int[len];
    rGlobalMax[len - 1] = rLocalMax;
    rGlobalMin[len - 1] = rLocalMin;
    
    for (int i = len - 2; i >= 0; i--) {
        rLocalMax = Math.max(nums.get(i), rLocalMax + nums.get(i));
        rGlobalMax[i] = Math.max(rGlobalMax[i+1], rLocalMax);
        rLocalMin = Math.min(nums.get(i), rLocalMin + nums.get(i));
        rGlobalMin[i] = Math.min(rGlobalMin[i+1], rLocalMin);
    }
    
    int res = 0;
    
    for (int i = 1; i < len - 1; i++) {
        if (res < Math.abs(lGlobalMin[i] - rGlobalMax[i+1])) {
            res = Math.abs(lGlobalMin[i] - rGlobalMax[i+1]);
        }
        if (res < Math.abs(lGlobalMax[i] - rGlobalMin[i+1])) {
            res = Math.abs(lGlobalMax[i] - rGlobalMin[i+1]);
        }
    }
    
    return res;

}
```

---
###Reference
1. http://www.fgdsb.com/2015/01/05/max-dif-of-two-subarrays/