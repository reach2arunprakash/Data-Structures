#Minimum Subarray

[]()

題意：此為Maximum Subarray的變形，給予一個陣列，找出一個子陣列其中的和是最小的

解題思路：使用兩個變數 cur 與 min 分別紀錄當前的和，與當前的最小值，會有以下兩個狀況。

+ 若 cur + nums[i] 比當前的 nums[i] 還來得大的話，代表目前值包含之前的和的話，總和會變大，之前的總和反而幫了倒忙，由於此題是求最小，因此放棄前面的總和，從 i 開始繼續再找。
+ 若 cur + nums[i] 比當前的 nums[i] 小的話，則代表之前的 cur有用，把cur留著與目前的 nums[i] 相加，接著比較一下有沒有比當前的 min小，若有，則更新。

```java
public int minSubArray(ArrayList<Integer> nums) {
    if (nums == null || nums.size() == 0) {
        return 0;
    }
    
    int min = nums.get(0);
    int cur = nums.get(0);
    // 與Maximum Subarray 大同小異，但在這裡要比的是當下的值是否比cur+當下值來得小，
    // 若是，則直接從當下值重新開始算
    // 若否，則把當下值累積繼續下一步
    for (int i = 1; i < nums.size(); i++) {
        if (nums.get(i) < cur + nums.get(i)) {
            cur = nums.get(i);
        } else {
            cur += nums.get(i);
        }
        min = Math.min(cur, min);
    }    
    return min;
}
```

>Time Complexity：$$O(N)$$，因只需遍歷一次陣列即可。