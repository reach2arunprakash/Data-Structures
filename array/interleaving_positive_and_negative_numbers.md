#Interleaving Positive and Negative Numbers

[原題網址](http://www.lintcode.com/en/problem/interleaving-positive-and-negative-numbers/)

題意：給定一陣列，排成正負數交錯。

解題思路：此題不容易，需要很小心的處理指針與正負個數不同的問題，參考了Yu Zhang網友的作法。

1. 把正數與負數分兩邊，預設正數在左邊，負數在右邊。
2. 接著檢查正數個數與負數個數，把較少的透過反轉換到前面。
3. 利用一次跳兩格的方式來把不符合規定的數來作交換。

程式碼如下：

```java
class Solution {
    /**
     * @param A: An integer array.
     * @return: void
     */
    public void rerange(int[] A) {
        
        if (A == null || A.length == 0) {
            return;
        }
        
        int len = A.length;
        int countPos = 0;
        int posIdx = 0;
        
        // 把所有正數放到前面
        for (int negIdx = 0; negIdx < len; negIdx++) {
            
            if (A[negIdx] > 0) {
                swap (A, posIdx, negIdx);
                posIdx++;
                countPos++;
            }
        }
        
        int posPointer  = 1;
        int negPointer = 0;
        
        // 如果正數個數較多，則把較少的負數放到前面來，並改變正負指針的起始位置
        // 原則就是把較少的數放在前面
        if (countPos > len / 2) {
            posPointer = 0;
            negPointer = 1;
            
            int left = 0;
            int right = len - 1;
            while (left < right) {
                swap(A, left, right);
                left++;
                right--;
            }
        }
        // 使用變形的partion 方法來把正負數交錯排列
        while (posPointer < len && negPointer < len) {
            while (posPointer < len && A[posPointer] >= 0) {
                posPointer += 2;
            }
            
            while (negPointer < len && A[negPointer] < 0) {
                negPointer += 2;
            }
            
            swap(A, negPointer, posPointer);
            negPointer += 2;
            posPointer += 2;
        }
        
        
   }
   
   
   
   private void swap(int[] A, int n1, int n2) {
       int temp = A[n1];
       A[n1] = A[n2];
       A[n2] = temp;
   }
}
```

>Time Complexity：$$O(N)$$
>Space Complexity：$$O(1)$$

---
###Reference
1. http://www.cnblogs.com/yuzhangcmu/p/4175620.html