# Shortest Word Distance II

[Leetcode](https://leetcode.com/problems/shortest-word-distance-ii/)

題意：

This is a follow up of Shortest Word Distance. The only difference is now you are given the list of words and your method will be called repeatedly many times with different parameters. How would you optimize it?

Design a class which receives a list of words in the constructor, and implements a method that takes two words word1 and word2 and return the shortest distance between these two words in the list.

For example,
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = 「coding」, word2 = 「practice」, return 3.
Given word1 = "makes", word2 = "coding", return 1.

Note:
You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.


解題思路：

使用hashmap把字的所有index放在一起，再一一比對找出最短的。

```java
public class WordDistance {
    
    HashMap<String, List<Integer>> map;
    public WordDistance(String[] words) {
        map = new HashMap<String, List<Integer>>();
        for (int i = 0; i < words.length; i++) {
            if (!map.containsKey(words[i])) {
                map.put(words[i], new ArrayList<Integer>());
            }
            map.get(words[i]).add(i);
        }
    }

    public int shortest(String word1, String word2) {
        List<Integer> listOne = map.get(word1);
        List<Integer> listTwo = map.get(word2);
        int minDistance = Integer.MAX_VALUE;
        for (int i = 0; i < listOne.size(); i++) {
            for (int j = 0; j < listTwo.size(); j++) {
                int distance = Math.abs(listOne.get(i) - listTwo.get(j));
                minDistance = Math.min(minDistance, distance);
            }
        }
        
        return minDistance;
    }
}

// Your WordDistance object will be instantiated and called as such:
// WordDistance wordDistance = new WordDistance(words);
// wordDistance.shortest("word1", "word2");
// wordDistance.shortest("anotherWord1", "anotherWord2");
```