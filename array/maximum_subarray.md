#Maximum Subarray

[]()

題意：給予一陣列，找出子陣列之和為最大

解題思路：此為 [Minimum Subarray](array/minimum_subarray.md) 的變形，使用相同方式即可。另有 Prefix Sum 方式也能解此題。

```java
public int maxSubArray(ArrayList<Integer> nums) {
    if (nums == null || nums.size() == 0) {
        return 0;
    }
    
    //解法：先把第一個數加入current與設為max
    //      接著就一直不斷的先判斷current再決定丟棄前面的結果
    //      或是原本結果加上新的數
    int max = nums.get(0);
    int current = nums.get(0);
    for (int i = 1; i < nums.size(); i++) {
        if (current < 0) {
            current = nums.get(i);
        } else {
            current += nums.get(i);
        }
        max = (current > max) ? current : max;
    }
    return max;
}
```

>Time Complexity：$$O(N)$$

---
###Prefix Sum 解法

解題思路：
利用一個 ```sum``` 陣列，```sum[i]``` 代表```nums[0] + ... + nums[i]```的和，若要求```sum[i..j]```的話，直接求 ```sum[j] - sum[i - 1]```即可。空間複雜度為$$O(N)$$，下面程式解法只利用一個```sum```與```minSum```變數來記錄當前的最小下限為多少，利用```minSum```來當作```sum[i-1]```，```sum```即為```sum[j]```。

```java
public int maxSubArray(int[] A) {
    if (A == null || A.length == 0){
        return 0;
    }
    
    int max = Integer.MIN_VALUE, sum = 0, minSum = 0;
    for (int i = 0; i < A.length; i++) {
        sum += A[i];
        max = Math.max(max, sum - minSum);
        minSum = Math.min(minSum, sum);
    }

    return max;
}
```
>Time Complexity：$$O(N)$$