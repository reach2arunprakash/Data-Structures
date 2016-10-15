# Shortest Word Distance

[Leetcode](https://leetcode.com/problems/shortest-word-distance/)


題意：

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

For example,
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = “coding”, word2 = “practice”, return 3.
Given word1 = "makes", word2 = "coding", return 1.

Note:
You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.


解題思路：

updated 2016.1.7

較naive，但程式較複雜點，為什麼找到了還要繼續判斷而不直接返回，是因為陣列中面能含有兩個以上相同的字串。

```java
public class Solution {
    public int shortestDistance(String[] words, String word1, String word2) {
        if (words == null || words.length < 2) {
            return 0;
        }
        
        int idxOne = -1;
        int idxTwo = -1;
        int minLength = Integer.MAX_VALUE;
        
        for (int i = 0; i < words.length; i++) {
            if (words[i].equals(word1)) {
                idxOne = i;
            } else if (words[i].equals(word2)) {
                idxTwo = i;
            }
            
            // 為什麼找到了還要繼續判斷而不直接返回，是因為陣列中面能含有兩個以上相同的字串
            if (idxOne != -1 && idxTwo != -1) {
                minLength = Math.min(minLength, Math.abs(idxOne - idxTwo));
            }
        }
        
        return minLength;
    }
}
```


只使用一個index來標記在當下字串之前遇到的另一個字的index為多少，記得再判斷index指的那個字與當下的字不同，再更新mindistance。
```java
public class Solution {
    public int shortestDistance(String[] words, String word1, String word2) {
        int index = -1;
        int minDistance = Integer.MAX_VALUE;
        for (int i = 0; i < words.length; i++) {
            if (words[i].equals(word1) || words[i].equals(word2)) {
                if (index != -1 && !words[index].equals(words[i])) {
                    minDistance = Math.min(minDistance, i - index);
                } 
                index = i;
            }
        }
        
        return minDistance;
    }
}
```

---
###Reference
1. https://leetcode.com/discuss/61820/java-only-need-to-keep-one-index