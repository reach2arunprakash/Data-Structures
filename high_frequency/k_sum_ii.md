# K Sum II

[Lintcode](http://www.lintcode.com/en/problem/k-sum-ii/)

題意：

Given n unique integers, number k (1<=k<=n)  and target. Find all possible k integers where their sum is target.


解題思路：

使用 dfs 來解，其實這題就是[Combination Sum](high_frequency/combination_sum.md) 的變形，但我們不需要略過重複的數，只要確定當下的結果元素個數等於k且和為target即可。

程式碼如下：

```java
public class Solution {
    /**
     * @param A: an integer array.
     * @param k: a positive integer (k <= length(A))
     * @param target: a integer
     * @return a list of lists of integer 
     */
    ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
    
    public ArrayList<ArrayList<Integer>> kSumII(int A[], int k, int target) {
        if (A == null || A.length == 0 || target < 0) {
            return res;
        }
        
        helper(new ArrayList<Integer>(), A, k , target, 0);
        return res;
    }
    
    private void helper(ArrayList<Integer> tempRes, int[] A, int k, int remain, int pos) {
        if (tempRes.size() == k) {
            if (remain == 0) {
                res.add(new ArrayList<Integer>(tempRes));
            }
            return;
        } else {
            for (int i = pos; i < A.length; i++) {
                tempRes.add(A[i]);
                helper(tempRes, A, k, remain - A[i], i + 1);
                tempRes.remove(tempRes.size() - 1);
            }
        }
    }
}
```

---
###Reference
1. http://www.cnblogs.com/EdwardLiu/p/4314783.html