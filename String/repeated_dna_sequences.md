# Repeated DNA Sequences

[Leetcode](https://leetcode.com/problems/repeated-dna-sequences/)

題意：

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

For example,
```
Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",

Return:
["AAAAACCCCC", "CCCCCAAAAA"].
```

解題思路：

暴力法，把每十個字元存成一個字串放在hashset中，再一一比對。非常耗時。


hashset加bit manipulation法

>A = 00，C = 01，G = 10，T = 11。
>
int key = 0, key = key << 2 | code(A|C|G|T)。

因為我們當第i-(i+9)的字串要改為(i+1)-(i+10)的字串時，只需要去掉第i個字元並加入第(i+10)個字元即可，我們可以利用bit manipulation用shift的方式並or起來即可，非常省時。

程式碼如下：

```java
public class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        List<String> res = new ArrayList<String>();
        if (s == null || s.length() < 10) {
            return res;
        }
        
        HashSet<Integer> set = new HashSet<Integer>();
        int len = s.length();
        for (int i = 0; i <= len - 10; i++) {
            String str = s.substring(i, i + 10);
            int key = genKey(str);
            if (set.contains(key) && !res.contains(str)) {
                res.add(str);
            } else {
                set.add(key);
            }
        }
        
        return res;
    }
    
    private int genKey(String str) {
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        map.put('A', 0);
        map.put('C', 1);
        map.put('G', 2);
        map.put('T', 3);
        
        int res = 0;
        for (int i = 0; i < str.length(); i++) {
            res = res << 2 | map.get(str.charAt(i));
        }
        return res;
    }
}
```
---
###Reference
1. http://blog.csdn.net/wzy_1988/article/details/44224749