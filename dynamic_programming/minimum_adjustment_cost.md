# Minimum Adjustment Cost

[Lintcode](http://www.lintcode.com/en/problem/minimum-adjustment-cost/)

題意：

Given an integer array, adjust each integers so that the difference of every adjacent integers are not greater than a given number target.

If the array before adjustment is A, the array after adjustment is B, you should minimize the sum of |A[i]-B[i]|

Example

Given [1,4,2,3] and target = 1, one of the solutions is [2,3,2,3], the adjustment cost is 2 and it's minimal.

Return 2.

>即給一個陣列與一個 target，試著調整每個陣列元素使得與鄰近的陣列元素**小於** target值，但求最小的調整成本。

解題思路：

由於題目已規定值只有可能從1到100，我們可以根據每個的index去枚舉1到100的可能性，可使用遞迴來幫助我們，但此題複雜度太高了，為指數性的複雜度，其程式碼如下：

```java
public class Solution {
    /**
     * @param A: An integer array.
     * @param target: An integer.
     */
    public int MinAdjustmentCost(ArrayList<Integer> A, int target) {
        if (A == null) {
            return 0;
        }
        
        return helper(A, new ArrayList<Integer>(A), target, 0);
    }
    
    public int helper(ArrayList<Integer> A, ArrayList<Integer> B, int target, int index) {
        int len = A.size();
        if (index >= len) {
            return 0;
        }
        
        int diff = 0;
        
        int min = Integer.MAX_VALUE;
        for (int i = 0; i <= 100; i++) {
            if (index != 0 && Math.abs(i - B.get(index - 1)) > target) {
                continue;
            }
            B.set(index, i);
            diff = Math.abs(i - A.get(index));
            diff += helper(A, B, target, index + 1);
            min = Math.min(min, diff);
            
            B.set(index, A.get(index));
        }
        
        return min;
    }
}
```

不過我們可以透過memory search來避免重複的運算，其程式碼如下：

>M[i][j] 表示該該陣列第i個元素為j的最小成本為多少。

>M[i][j]的定義是：從index = i處開始往後所有的differ，並且A[i]的取值取為j + 1;

```
public class Solution {
    /**
     * @param A: An integer array.
     * @param target: An integer.
     */
    int[][] M;
    public int MinAdjustmentCost(ArrayList<Integer> A, int target) {
        if (A == null) {
            return 0;
        }
        M = new int[A.size()][100];
        for (int i = 0; i < A.size(); i++) {
            for (int j = 0; j < 100; j++) {
                M[i][j] = Integer.MAX_VALUE;
            }
        }
        return helper(A, new ArrayList<Integer>(A), target, 0);
    }
    
    public int helper(ArrayList<Integer> A, ArrayList<Integer> B, int target, int index) {
        int len = A.size();
        if (index >= len) {
            return 0;
        }
        
        int diff = 0;
        
        int min = Integer.MAX_VALUE;
        for (int i = 1; i <= 100; i++) {
            if (index != 0 && Math.abs(i - B.get(index - 1)) > target) {
                continue;
            }
            
            // 只要之前計算過了，直接返回
            if (M[index][i - 1] != Integer.MAX_VALUE) {
                diff = M[index][i - 1];
                min = Math.min(min, diff);
                continue;
            }
            B.set(index, i);
            diff = Math.abs(i - A.get(index));
            
            diff += helper(A, B, target, index + 1);
            min = Math.min(min, diff);
            M[index][i - 1] = diff; //紀錄在該index而值為i的最小成本為多少
            
            B.set(index, A.get(index));
        }
        
        return min;
    }
}
```

最後我們可以透過動態規劃來達到

引用網友 [Yu Zhang](http://www.cnblogs.com/yuzhangcmu/p/4153927.html) 的解說
>f[i][v] 前i個數，第i個數調整為v，滿足相鄰兩數<= target，所需要的最小代價
>
**即把index = i的值修改為v，所需要的最小花費**

>function: f[i][v] = min(f[i-1][v'] + |A[i]-v|, |v-v'| <= target)

就是當前index為v時，我們把上一個index從1-100全部過一次，取其中的最小值（判斷一下前一個跟當前的是不是abs <= target）

其程式碼如下：

```java
public class Solution {
    /**
     * @param A: An integer array.
     * @param target: An integer.
     */
    public int MinAdjustmentCost(ArrayList<Integer> A, int target) {
        
        if (A == null || A.size() == 0) {
            return 0;
        }
        
        int[][] f = new int[A.size()][101]; //第1行不使用，讓code比較簡潔點
        
        int len = A.size();
        
        for (int i = 0; i <len; i++) {
            for (int j = 1; j <= 100; j++) {
                f[i][j] = Integer.MAX_VALUE;
                if (i == 0) {
                    f[i][j] = Math.abs(j - A.get(i)); // 因是第一位數，只需要知道把改成j的成本為多少即可
                } else {
                    for (int k = 1; k <= 100; k++) {
                        if (Math.abs(j - k) > target) {
                            continue;
                        }
                        
                        int diff = Math.abs(j - A.get(i)) + f[i-1][k];
                        f[i][j] = Math.min(f[i][j], diff);
                    }
                }
            }
        }
        
        // 因只計算了最後一位數各為1-100的最小成本為多少，因此需要一個for loop來找出最小值
        int min = Integer.MAX_VALUE;
        for (int i = 1; i <= 100; i++) {
            min = Math.min(min, f[len - 1][i]);
        }
        
        return min;
    }
}
```

---
#Reference
1. http://www.cnblogs.com/yuzhangcmu/p/4153927.html

