#Continuous Subarray Sum II

[原題網址](http://www.lintcode.com/en/problem/continuous-subarray-sum-ii/)

題意：與 I 相同，但這次陣列可以旋轉。

Give ```[3, 1, -100, -3, 4]```, return ```[4,1]```.

解題思路：

網友 stellari 提供以下思路：

最大數組的範圍只有兩種可能：
1. [ i ~ j ]
2. [ i ~ N-1] + [ 0 ~ j ]. 

所以，你只要分別找到兩種情況的最大者，取這兩個最大者中較大的即可。

1和Continuous Subarray Sum I相同，就不多說了。

2等價於找一個範圍[j+1 ~i-1]，使得這個範圍內的數組和最小。這又等價於將原數組取負號，然後在這個負數組中找最大和的[j+1 ~ i+1]範圍即可。


網友 momocoisapeach 亦提供以下思路：

思路是這樣的，像樓上說的兩種情況，不rotate的 和rorate的。不rotate的和Continuous Subarray Sum I做法一樣，不說了。rotate的，可以這樣想，rotate的結果其實相當於是把原來的array中間挖了一段連續的array，那麼挖哪一段呢？肯定是和最小的一段連續array。這樣解法就出來了。"

類似Continuous Subarray Sum I，在I裡面是找到連續和最大的一段subarray，在這裡，不僅找到和最大的一段連續array，並且也找到和最小的一段連續array，然後用整個array的sum減去這個最小的和，如果大於不rotate的最大和，那麼解就是挖去的這段array的（尾+1， 頭-1)

程式碼如下：

```java
public class Solution {
    /**
     * @param A an integer array
     * @return  A list of integers includes the index of the first number and the index of the last number
     */
    public ArrayList<Integer> continuousSubarraySumII(int[] A) {
        
        ArrayList<Integer> res = new ArrayList<Integer>();
        if (A == null || A.length == 0) {
            return res;
        }
        
        int start = 0;
        int end = 0;
        int sum = A[0];
        int max = A[0];
        int totalSum = A[0];
        res.add(0);
        res.add(0);
        
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
            
            totalSum += A[i]; // for latter use.
        }
        
        // 主要目的是中間找一段最小的，然後把 totalsum 減掉即為 rotate的 連續子矩陣和
        // 把矩陣中所有正元素轉負，負元素轉正，接著再依前面的方法來求最大即可
        int[] negativeA = new int[A.length];
        for (int i = 0; i < A.length; i++) {
            negativeA[i] = -A[i];
        }
        
        start = 0;
        end = 0;
        sum = negativeA[0];
        for (int i = 1; i < negativeA.length; i++) {
            if (sum < 0) {
                start = i;
                sum = negativeA[i];
            } else {
                sum += negativeA[i];
            }
            end = i;
            
            // 應是 total sum - minSum，但由於矩陣已正負交換，所以為totalSum - (-minSum)
            if (totalSum + sum > max) {
                max = totalSum + sum;
                res.set(0, end + 1);
                res.set(1, start - 1);
            }
        }
        
        return res;
    }
}
```

>Time Complexity：$$O(N)$$，Space Complexity：$$O(N)$$ for negative Array.

---

網友 [glaciersilent](http://www.1point3acres.com/bbs/forum.php?mod=viewthread&action=printable&tid=137556) 提供了以下方法，使用兩個array 來達到one pass通過，程式碼如下：

```java
public class Solution {
    /**
     * @param A an integer array
     * @return  A list of integers includes the index of the first number and the index of the last number
     */
    public ArrayList<Integer> continuousSubarraySumII(int[] A) {
        
        if (A == null || A.length == 0) {
            return new ArrayList<Integer>();
        }
        
        int len = A.length;
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        ArrayList<Integer> maxIndex = new ArrayList<Integer>();
        ArrayList<Integer> minIndex = new ArrayList<Integer>();
        maxIndex.add(0);
        maxIndex.add(0);
        minIndex.add(0);
        minIndex.add(0);
        
        int[] maxArray = new int[len];
        int[] minArray = new int[len];
        int[] maxStart = new int[len];
        int[] minStart = new int[len];
        maxArray[0] = A[0];
        minArray[0] = A[0];
        int sum = A[0];
        
        for (int i = 1; i < len; i++) {
            sum += A[i];
            if (maxArray[i - 1] > 0) {
                maxArray[i] = maxArray[i - 1] + A[i];
                maxStart[i] = maxStart[i - 1];
            } else {
                maxArray[i] = A[i];
                maxStart[i] = i;
            }
            
            if (minArray[i - 1] < 0) {
                minArray[i] = minArray[i - 1] + A[i];
                minStart[i] = minStart[i - 1];
            } else {
                minArray[i] = A[i];
                minStart[i] = i;
            }
            
            if (maxArray[i] > max) {
                max = maxArray[i];
                maxIndex.set(0, maxStart[i]);
                maxIndex.set(1, i);
            }
            
            if (minArray[i] < min) {
                min = minArray[i];
                minIndex.set(0, (i + 1) % len);
                minIndex.set(1, minStart[i] - 1 >= 0 ? minStart[i] - 1 : A.length - 1);
            }
        }
        
        if (max > sum - min || min == sum) {
            return maxIndex;
        } else {
            return minIndex;
        }
    }
}

```
---
###Reference
1. http://www.1point3acres.com/bbs/forum.php?mod=viewthread&action=printable&tid=137556