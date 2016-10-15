#Continuous Subarray Sum

[原題網址](http://www.lintcode.com/en/problem/continuous-subarray-sum/)

題意：給一矩陣，找出最大的連續子矩陣和。

解題思路：

後來找到網友[Techinpad](http://techinpad.blogspot.com/2015/06/lintcode-continuous-subarray-sum.html) 提供的解法較容易懂，類似把改良版的 subarray sum，其程式碼如下：

sum 表示必含 A[i] 的最大和為多少

```java
public ArrayList<Integer> continuousSubarraySum(int[] A) {
        
    ArrayList<Integer> res = new ArrayList<Integer>();
    if (A == null || A.length == 0) {
        return res;
    }
    
    int start = 0;
    int end = 0;
    int sum = A[0];
    int max = A[0];
    res.add(0); //start
    res.add(0); //end
    
    for (int i = 1; i < A.length; i++) {
        if (sum < 0) {
            start = i;
            sum = A[i];
        } else {
            sum += A[i];
        }
        end = i;
        
        if (sum > max) {
            max = sum;
            res.set(0, start);
            res.set(1, end);
        }
    }
    
    return res;
}
```

---

使用 sum 來紀錄當前總和，max來紀錄目前最大的總和，若sum > max 則代表找到更好的子矩陣，更新它。需注意若當前加起來的值小於0，捨棄前面的總和，從當前的元素開始，需特別注意的地方是我們總是在下一次才檢查是否更新上一次的結果，因此 for 循環結束後，我們需要再檢查一次，其代碼如下：

程式碼如下：

```java
public ArrayList<Integer> continuousSubarraySum(int[] A) {
    
    ArrayList<Integer> res = new ArrayList<Integer>();
    
    if (A == null || A.length == 0) {
        return res;
    }
    
    // 先假設目前最大值為第一個元素
    int sum = A[0];
    int max = sum;
    int start = 0;
    int end = 0;
    res.add(0);
    res.add(0);
    
    for (int i = 1; i < A.length; i++) {
        if (sum > max) {
            res.set(0, start);
            res.set(1, i - 1);
            max = sum;
        }
        
        //如果加起來更小的話，則捨棄前面的和，直接從目前這個元素開始
        if (sum < 0) {
            sum = 0; 
            start = i;
            end = i;
        }
        sum += A[i];
    }
    
    // 因上面的循環總是在下一次才把上一輪的結果加上，因此最後還需再判斷一次。
    if (sum > max) {
        res.set(0, start);
        res.set(1, A.length - 1);
    }
    
    return res;
}
```
>Time Complexity：$$O(N)$$

---
###Reference
1. http://cherylintcode.blogspot.com/2015/07/continuous-subarray-sum.html
2. http://techinpad.blogspot.com/2015/06/lintcode-continuous-subarray-sum.html