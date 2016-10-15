#Permutation Sequence

[Lintcode](http://www.lintcode.com/en/problem/permutation-sequence/)

題意：

Given n and k, return the k-th permutation sequence.

**Example**

For ```n = 3```, all permutations are listed as follows:
```
"123"
"132"
"213"
"231"
"312"
"321"
```
If ```k = 4```, the fourth permutation is ```"231"```

Note
n will be between 1 and 9 inclusive.

**Challenge**

O(n*k) in time complexity is easy, can you do it in O(n^2) or less?

解題思路：

DFS法：用DFS把所有可能全列出來，再返回第k個值，時間複雜度太高，程式碼如下：

```java
class Solution {
    /**
      * @param n: n
      * @param k: the kth permutation
      * @return: return the k-th permutation
      */
      
    List<String> res;
    boolean[] visited;
    public String getPermutation(int n, int k) {
        if (n == 0) {
            return "";
        }
        
        res = new ArrayList<String>();
        visited = new boolean[n];
        helper(n, new StringBuilder());
        return res.get((k - 1));
    }
    
    public void helper(int n, StringBuilder sb) {
        if (sb.length() == n) {
            res.add(sb.toString());
        }
        
        for (int i = 1; i <= n; i++) {
            if (visited[i - 1] == false) {
                sb.append(String.valueOf(i));
                visited[i - 1] = true;
                helper(n, sb);
                sb.deleteCharAt(sb.length() - 1);
                visited[i - 1] = false;
            }
        }
    }
}


```

法二：

參考網友 [tso](https://leetcode.com/discuss/42700/explain-like-im-five-java-solution-in-o-n) 的解法，不斷的找出每個位元應該是什麼數，但仍超時。

```java
public class Solution {
    public String getPermutation(int n, int k) {

        List<Integer> numbers = new ArrayList<>();
        int[] factorial = new int[n + 1];
        StringBuilder sb = new StringBuilder();
        
        // create an array of factorial lookup
        // factorial[] = {1, 1, 2, 6, 24, ... n!};
        int product = 1;
        factorial[0] = 1;
        for (int i = 1; i <= n; i++) {
            product *= i;
            factorial[i] = product;
        }
        
        // create a list of numbers to get indices
        for (int i = 1; i <= n; i++) {
            numbers.add(i);
        }
        
        k--;
        
        for (int i = 1; i <= n; i++) {
            int index = k / factorial[n - i];
            int val = numbers.get(index);
            sb.append(val);
            k -= index * factorial[n - i];
            numbers.remove(index); // 重要
        }
        
        return String.valueOf(sb);
    }
}
```

與上面類似的解法，但accept：

```java
public class Solution {
    public String getPermutation(int n, int k){
        ArrayList<Integer> list = new ArrayList<Integer>();
        StringBuilder sb = new StringBuilder();
        for(int i=1; i<=n; i++) {
            list.add(i);
        }
        int nth = k-1;
        int divider = factorial(n-1);  
        while(n>1) {
            int index = nth/divider;
            nth = nth%divider;
            sb.append(list.get(index));
            list.remove(index);
            divider /= n-1;
            n--;            
        }
        sb.append(list.get(0));// append last digit
    
        return sb.toString();
    }
    
    public int factorial(int n) {  // use tail recursion
        return fact(n, 1);
    }
    private int fact(int n, int k) {
        if(n==0)
            return k;
        else {
            return fact(n-1, n*k);
        }
    }
}
```
另有數學解法：


---
###Reference
1. http://blog.hfknight.com/?p=1303
2. https://leetcode.com/discuss/42700/explain-like-im-five-java-solution-in-o-n
