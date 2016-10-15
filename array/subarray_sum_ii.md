#Subarray Sum II

題意：

給定一個矩陣，與一個 range，找出該矩陣所有符合該 range 的 子矩陣和的個數。

Example
Given [1,2,3,4] and interval = [1,3], return 4. The possible answers are:
```
[0, 0]
[0, 1]
[1, 1]
[2, 2]
```

解題思路：先預處理一個 sumArray來存放前i個元素和，接著再枚舉所有可能的搭配，若 sum[i] - sum[j] 在範圍內的話，則 count 加一，其代碼如下：

```java
public int subarraySumII(int[] A, int start, int end) {
    
    if (A == null || A.length == 0) {
        return 0;
    }
    
    //sumArray[i] 表示前i個元素和
    int[] sumArray = new int[A.length];
    sumArray[0] = 0; // 為了避免第一個元素在range裡，因此加了此元素。
    int sum = 0;
    for (int i = 1; i <= A.length; i++) {
        sum += A[i - 1];
        sumArray[i] = sum;
    }
    
    int count = 0;
    for (int i = 0; i <= A.length; i++) {
        for (int j = i; j <= A.length; j++) {
            int diff = sumArray[j] - sumArray[i];
            if (diff >= start && diff <= end) {
                count++;
            }
        }
    }
    
    return count;
}
```
---
Time Complexity：$$O(N^{2})$$