#Print Numbers by Recursion

[原題網址](http://www.lintcode.com/en/problem/print-numbers-by-recursion/)

題意：給一個數 n 代表位數，印出所有小於該位數的所有數字。

Print numbers from 1 to the largest number with N digits by recursion.

Example

Given N = 1, return ```[1,2,3,4,5,6,7,8,9]```.

Given N = 2, return ```[1,2,3,4,5,6,7,8,9,10,11,12,...,99]```.

解題思路：

暴力法：此法複雜度高，會遞迴太多層，如 n = 4，四位數，則需遞迴1000層。

```java
public class Solution {
    /**
     * @param n: An integer.
     * return : An array storing 1 to the largest number with n digits.
     */
    public List<Integer> numbersByRecursion(int n) {
        
        int digits = 10;
        for (int i = 1; i < n; i++) {
            digits *= 10;
        }
        
        List<Integer> res = new ArrayList<Integer>();
        if (n == 0) {
            return res;
        }
        
        helper(1, digits, res);
        return res;
    }
    
    public void helper(int i, int digits, List<Integer> res) {
        if (i >= digits) {
            return;
        }
        res.add(i);
        helper(i + 1, digits, res);
    }
}

```

改良後的版本，由[網友](https://codesolutiony.wordpress.com/2015/05/21/lintcode-print-numbers-by-recursion/)提供，每一位數只遞迴一層，如四位數則只遞迴四層(0位數，個位，十位，百位)。

```java
public class Solution {
    /**
     * @param n: An integer.
     * return : An array storing 1 to the largest number with n digits.
     */
    public List<Integer> numbersByRecursion(int n) {
        
        List<Integer> res = new ArrayList<Integer>();
        if (n > 0) {
            helper(n, res);
        }
        return res;
    }
    
    private int helper(int n, List<Integer> res) {
        if (n == 0) {
            return 1;
        }
        
        int base = helper(n - 1, res);
        int size = res.size();
        
        
        // i 枚舉所有該base位數。
        // 如 base = 100 則 i 則枚舉 100, 200, 300, ..., 900。
        for (int i = 1; i <= 9; i++) { // 枚舉所有該base位數。
            int curBase = base * i;
            res.add(curBase);
            // 不斷的把之前存在list 中的數拿出來加上新的base則為新的數
            // 假設原本 list 為{1, ..., 99}
            // 則經過這層處理後會變成 {1, ..., 99, 100, 101, 102, ..., 998, 999}
            for (int j = 0; j < size; j++) {
                res.add(curBase + res.get(j));
            }
        }
        
        // 因base是從下一層傳回來的，
        // 因此要回傳當前層的base需再乘10
        return base * 10; 
    }
}

```