# Permutation Index II

[]()

題意：

為 [Permutation Index](math/permutation_index.md) 的 follow up，主要要考慮重複的元素，若有重複元素，需要在原本的index上除以重複元素個數的階乘，在這裡我們使用一個hash map來保存目前有幾個元素重複了。

> 計算重複元素個數的階乘需要乘 dup * fac(val)，而非 dup *= val

程式碼如下：

```java
public class Solution {
    /**
     * @param A an integer array
     * @return a long integer
     */
    public long permutationIndexII(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        
        long index = 1;
        long factor = 1;
        for (int i = A.length - 2; i >= 0; i--) {
            HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
            map.put(A[i], 1);
            int rank = 0;
            for (int j = i + 1; j < A.length; j++) {
                if (!map.containsKey(A[j])) {
                    map.put(A[j], 1);
                } else {
                    map.put(A[j], map.get(A[j]) + 1);
                }
                if (A[i] > A[j]) {
                    rank++;
                }
            }
            index += rank * factor / dupPermNum(map);
            factor *= (A.length - i);
        }
        return index;
    }
    
    private long dupPermNum(HashMap<Integer, Integer> map) {
        if (map == null || map.isEmpty()) {
            return 1;
        }
        
        long dup = 1;
        for (int val : map.values()) {
            dup *= fac(val);
        }
        
        return dup;
    }
    
    private long fac(int val) {
        int res = 1;
        for (int i = 1; i <= val; i++) {
            res *= i;
        }
        return res;
    }
}
```

>Time Complexity：$$O(N^2)$$，Space Complexity：O(N)

---
###Reference
1. http://algorithm.yuanbin.me/zh-cn/exhaustive_search/permutation_index_ii.html

