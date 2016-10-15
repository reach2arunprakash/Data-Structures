# Shortest Word Distance III

[Leetcode](https://leetcode.com/problems/shortest-word-distance-iii/)


題意：

This is a follow up of Shortest Word Distance. The only difference is now word1 could be the same as word2.

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

word1 and word2 may be the same and they represent two individual words in the list.

For example,
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = 「makes」, word2 = 「coding」, return 1.
Given word1 = "makes", word2 = "makes", return 3.

Note:
You may assume word1 and word2 are both in the list.



解題思路：

與1相同，但此時word1可能與word2相同，其程式碼大致相同，只有在第8行的判稍作改變，程式碼如下：


```java
public class Solution {
    public int shortestWordDistance(String[] words, String word1, String word2) {
        int index = -1;
        int minDistance = Integer.MAX_VALUE;
        for (int i = 0; i < words.length; i++) {
            if (words[i].equals(word1) || words[i].equals(word2)) {
                if (index != -1) {
                    if (index != i && (!words[index].equals(words[i]) || word1.equals(word2))) {
                        minDistance = Math.min(minDistance, i - index);
                    }
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
1. https://leetcode.com/discuss/50715/12-16-lines-java-c