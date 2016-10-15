# Substring with Concatenation of All Words

[Leetcode](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

題意：

You are given a string, s, and a list of words, words, that are all of the same length. Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.

For example, given:

s: "barfoothefoobarman"

words: ["foo", "bar"]

You should return the indices: [0,9].

解題思路：

其實與Longest Substring without Repeating Character差不多，只是這次char變成string且可以有重複的數。




```java
public class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> res = new ArrayList<Integer>();
        if (s == null || s.length() == 0 || words == null || words.length == 0) {
            return res;
        }
        
        int wordLen = words[0].length();
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        HashMap<String, Integer> curMap = new HashMap<String, Integer>();
        
        //先把words所有元素個數紀錄一次放到map中
        for (int i = 0; i < words.length; i++) {
            if (map.containsKey(words[i])) {
                map.put(words[i], map.get(words[i]) + 1);
            } else {
                map.put(words[i], 1);
            }
        }
        
        // 枚舉每個words中的元素
        for (int i = 0; i < words.length; i++) {
            curMap.clear(); //清除之前的紀錄繼續往下
            
            int count = 0;
            int left = i; // 重要，表示此substring的頭, 如果透過這個頭往後可以找出所有值，則把left加入
            
            // 從第i個元素往後枚舉, 終止條件為 j-worlen
            for (int j = i; j <= s.length() - wordLen; j = j + wordLen) {
                String str = s.substring(j, j + wordLen);
                
                
                // 有含這個字才有戲唱，否則直接往下一個字取
                if (map.containsKey(str)){
                    
                    // 把每個元素都放到curmap中
                    if (curMap.containsKey(str)) {
                        curMap.put(str, curMap.get(str) + 1);
                    } else {
                        curMap.put(str, 1);
                    }
                    
                    // 檢查如果元素個數還不夠的話，且words中需要這個字，則COUNT++
                    // 且我們先更新curmap的，所以得包含等於的部份
                    if (curMap.get(str) <= map.get(str)) {
                        count++;
                    } else {
                        //如果超出這個次數的話，表示有進步的空間，我們借由不斷往前推進left來作
                        while (curMap.get(str) > map.get(str)) {
                            String temp = s.substring(left, left + wordLen); // 檢查目前將要刪除的字需不需要
                            if (curMap.containsKey(temp)) {
                                curMap.put(temp, curMap.get(temp) - 1);
                                if (curMap.get(temp) < map.get(temp)) {
                                    count--; //所需要的元素個數增加了，如果大於的話，表示還夠包含所有words中的元素，不需更新count
                                }
                            }
                            left += wordLen; // 往右推進left，並捨棄那個字
                        }
                    }
                    
                    // 用count來判所是否已滿足所有要求了，若是的話，把結果加進
                    if (count == words.length) {
                        res.add(left);
                        String temp = s.substring(left, left + wordLen);
                        if (curMap.containsKey(temp)) {
                            curMap.put(temp, curMap.get(temp) - 1);
                        }
                        count--;
                        left += wordLen;
                    } 
                    
                } else { // 沒比對到，直接到下一個繼續走
                    curMap.clear();
                    count = 0;
                    left = j + wordLen;
                }
                
            }
        }
        
        return res;
        
    }
}
```
---
###Reference
1. http://blog.csdn.net/linhuanmars/article/details/20343903