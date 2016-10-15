# 3 Sum Closest

[原題網址](http://www.lintcode.com/en/problem/3-sum-closest/)

題意：給於一個陣列，與一個目標值，找出陣列中任三元素和最接近目標值

解題思路：類似 3 Sum，需要不斷檢查元素和與目標值的差值，若較小則更新結果。需要注意的是移動前後指針時，只需移動其中之一，因是找最接近的，如果兩個指針都移動的話，可能會遺漏掉可能的結果，程式碼如下：


```java
public int threeSumClosest(int[] numbers ,int target) {
    
    if (numbers == null || numbers.length == 0) {
        return 0;
    }
    
    // 解法類似3sum，只是需要用兩個變數來判斷最接近的值，
    // 固定一點i，接著後面的數組用雙指針來一一比對
    Arrays.sort(numbers);
    int minDiff = Integer.MAX_VALUE;
    int result = 0;
    
    for (int i = 0; i < numbers.length - 2; i++) {
        
        int start = i + 1;
        int end = numbers.length - 1;;
        
        while (start < end) {
            
            int curSum = numbers[i] + numbers[start] + numbers[end];
            int diff = Math.abs(target - curSum);
            
            if (diff < minDiff) {
                result = curSum;
                minDiff = diff;
            }
            
            // 只移動單一pointer，與3Sum不同，因此題是找最接近的，
            // 如果兩個pointer皆移動的話，可能會遺漏可能的結果。
            if (curSum > target) {
                end--;
            } else {
                start++;
            }
        }
    }
    
    return result;
}
```