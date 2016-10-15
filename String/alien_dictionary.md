# Alien Dictionary

[Leetcode](https://leetcode.com/problems/alien-dictionary/)

題意：

即給一段排序好的字串，找出字元的順序。

There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of words from the dictionary, where words are sorted lexicographically by the rules of this new language. Derive the order of letters in this language.

For example,
Given the following words in dictionary,
```
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]
```
The correct order is: "wertf".

Note:
You may assume all letters are in lowercase.
If the order is invalid, return an empty string.
There may be multiple valid order of letters, return any one of them is fine.

解題思路：

使用一個res stringbuilder來存放最終結果，temp stringbuilder來存放當下的結果，從每個字的第一個字元開始檢查，只要不在 temp中，且不在res中，則加入temp，最後把temp append到res中，繼續檢查第二個字元，以此類推，直到檢查完所有。

不知為什麼只通過一半的case，待修正後再更新，先附上目前的程式碼：

```java
public class Solution {
    public String alienOrder(String[] words) {
        StringBuilder sb = new StringBuilder();
        if (words == null || words.length == 0) {
            return sb.toString();
        }
        
        int len = maxLen(words);
        for (int i = 0; i < len; i++) {
            StringBuilder temp = new StringBuilder();
            for (int j = 0; j < words.length; j++) {
                if (words[j].length() <= i) {
                    continue;
                }
                String cur = "" + words[j].charAt(i);
                if (sb.indexOf(cur) == -1 && temp.indexOf(cur) == -1) {
                    temp.append(cur);
                }
            }
            sb.append(temp);
        }
        
        return sb.toString();
    }
    
    public int maxLen(String[] words) {
        int maxLen = 0;
        for (int i = 0; i < words.length; i++) {
            maxLen = Math.max(maxLen, words[i].length());
        }
        
        return maxLen;
    }
}
```