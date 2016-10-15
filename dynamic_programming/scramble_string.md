#Scramble String

[原題網址](http://www.lintcode.com/en/problem/scramble-string/)

題意：

Given a string s1, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of s1 = "great":

```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```
To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node "gr" and swap its two children, it produces a scrambled string "rgeat".

```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
          ```
We say that "rgeat" is a scrambled string of "great".

Similarly, if we continue to swap the children of nodes "eat" and "at", it produces a scrambled string "rgtae".

```
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
      ```
We say that "rgtae" is a scrambled string of "great".

Given two strings s1 and s2 of the same length, determine if s2 is a scrambled string of s1.

解題思路：

>以下四種解法參考網友 [Yu Zhang](http://www.cnblogs.com/yuzhangcmu/p/4189152.html) 的解題思路

暴力法：

基本的思想就是：在S1上找到一個切割點，左邊長度為i, 右邊長為len - i。 

有2種情況表明它們是IsScramble

1. S1的左邊和S2的左邊是IsScramble， S1的右邊和S2的右邊是IsScramble
2. S1的左邊和S2的右邊是IsScramble， S1的右邊和S2的左邊是IsScramble （實際上是交換了S1的左右子樹）


其複雜度約 $$O(3^{n})$$，以下引自網友 stellari對複雜度的解釋：

看了你的不少文章，感覺收穫良多！只是有點小問題想請教：按照我的理解，那個遞歸算法在最差情況下應該是O(3^n)，而非O(n^2)。理由是：假設函數運行時間為f(n)，那麼由於在每次函數調用中都要考慮1~n之間的所有長度，並且正反都要檢查，所以有
f(n) = 2[f(1) + f(n-1)] +2[f(2) + f(n-2)] … + 2[f(n/2) + f(n/2+1)]. 易推得f(n+1) = 3(fn), 故f(n) = O(3^n)。當然這是最差情況下的時間複雜度。那麼你提到的O(n^2)，是否是通過其他數學方法得到的更tight的上限？歡迎探討！

>複雜度太高，可透過lintcode但無法通過leetcode！

```java
public class Solution {
    /**
     * @param s1 A string
     * @param s2 Another string
     * @return whether s2 is a scrambled string of s1
     */
    public boolean isScramble(String s1, String s2) {
        
        if (s1 == null && s2 == null) {
            return true;
        }
        
        if (s1.length() != s2.length()) {
            return false;
        }
        
        return helper(s1, s2);
    }
    
    public boolean helper(String s1, String s2) {
        if (s1.length() != s2.length()) {
            return false;
        }
        
        if (s1.length() == 1) {
            return s1.equals(s2);
        }
        
        int len = s1.length();
        for (int i = 1; i < s1.length(); i++) {
            // 檢查 S1的左邊是否和S2的左邊是IsScramble， S1的右邊是否和S2的右邊是IsScramble
            if (helper(s1.substring(0, i), s2.substring(0, i)) 
                && helper(s1.substring(i, len), s2.substring(i, len))) {
                    return true;
            }
            // 檢查 S1 的左邊是否和 S2 的右邊是 IsScramble ， S1的右邊是否和 S2 的左邊是 IsScramble
            if (helper(s1.substring(0, i), s2.substring(len - i, len)) 
                && helper(s1.substring(i, len), s2.substring(0, len - i))) {
                    return true;
            }
        }
        
        return false;
    }
}

```

解法二：**遞迴法加減枝**

 來避免掉不必要的遞迴，在作遞迴前，先將兩個字串作排序，若有不同的字元則直接返回 false ，否則繼續往下檢查，可通過leetcode，但面試官應該不夠滿意。

```java
private boolean helper(String s1, String s2) {
    if (s1.length() == 1) {
        return s1.equals(s2);
    }
    
    char[] s1Char = s1.toCharArray();
    Arrays.sort(s1Char);
    char[] s2Char = s2.toCharArray();
    Arrays.sort(s2Char);
    
    for (int i = 0; i < s1Char.length; i++) {
        if (s1Char[i] != s2Char[i]) {
            return false;
        }
    }
    
    
    int len = s1.length();
    for (int i = 1; i < len; i++) {
        if (helper(s1.substring(0, i), s2.substring(0, i))
            && helper(s1.substring(i, len), s2.substring(i, len))) {
                return true;
        }
        
        if (helper(s1.substring(0, i), s2.substring(len - i, len))
            && helper(s1.substring(i, len), s2.substring(0, len - i))) {
                return true;
            }
    }
    
    return false;
}
```

解法三：**加上記憶矩陣**

我們在遞歸中加上記憶矩陣，也可以減少重複運算，但是我們現在就改一下之前遞歸的結構以方便加上記憶矩陣，我們用index1記憶S1起始地址，index2記憶S2起始地址，len 表示字符串的長度。這樣我們可以用一個三維數組來記錄計算過的值，同樣可以通過leetcode的檢查。這個三維數組一個是$$O(N^{3})$$的複雜度，在每一個遞歸中，要從1-len地計算一次所有的子串，所以一共的複雜度是N^4

> 因我們並未對原本的字串作任何改變，因此我們使用起始點(index1, index2) 加上長度 (len) 來幫助我們處用子字串的部份。


```java
public class Solution {
    public boolean isScramble(String s1, String s2) {
        if (s1 == null || s2 == null || s1.length() != s2.length()) {
            return false;
        }
        
        int len = s1.length();
        int[][][] mem = new int[len][len][len];
        for (int i = 0; i < len; i++) {
            for (int j = 0; j < len; j++) {
                for (int k = 0; k < len; k++) {
                    mem[i][j][k] = -1;
                }
            }
        }
        
        return helper(s1, 0, s2, 0, len, mem);
    }
    
    private boolean helper(String s1, int index1, String s2, int index2, int len, int[][][] mem) {
    
        if (s1.length() == 1) {
            // 不能直接用 equals，因為我們沒有對原本的字串作任何改變。
            return s1.charAt(index1) == s2.charAt(index2);
        }
        
        int res = mem[index1][index2][len - 1];
        if (res != -1) {
            return (res == 1) ? true : false;
        }
        
        res = 0;
        
        for (int i = 1; i < len; i++) {
            if (helper(s1, index1, s2, index2, i, mem) && helper(s1, index1 + i, s2, index2 + i, len - i, mem)) {
                res = 1;
                break;
            }
            
            if (helper(s1, index1, s2, index2 + len - i, i, mem) && helper(s1, index1 + i, s2, index2, len - i, mem)) {
                res = 1;
                break;
            }
        }
        
        mem[index1][index2][len - 1] = res;
        return (res == 1) ? true : false;
    }
}
```
---
###Reference

1. http://www.cnblogs.com/yuzhangcmu/p/4189152.html
2. 
