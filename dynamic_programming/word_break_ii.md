# Word Break II

[Leetcode](https://leetcode.com/problems/word-break-ii/)

題意：

Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

For example, given
s = "catsanddog",
dict = ["cat", "cats", "and", "sand", "dog"].

A solution is ["cats and dog", "cat sand dog"].


解題思路：

使用暴力法解，每次加一個字元來判斷是否在字典裡，若是，則繼續往下遞迴，否則就結束。

但此法 leetcode會超時

```java
public class Solution {
    List<String> res = new ArrayList<String>();
    public List<String> wordBreak(String s, Set<String> wordDict) {
        if (s == null || wordDict == null || wordDict.size() == 0) {
            return res;
        }
        helper(s, wordDict, new String(), 0);
        return res;
    }
    
    private void helper(String s, Set<String> wordDict, String temp, int start) {
        if (start >= s.length()) {
            res.add(temp);
            return;
        }
        
        StringBuilder sb = new StringBuilder();
        for (int i = start; i < s.length(); i++) {
            sb.append(s.charAt(i));
            if (wordDict.contains(sb.toString())) {
                if (temp.length() == 0) {
                    temp = sb.toString();
                } else {
                    temp = temp + " " + sb.toString();
                }
                helper(s, wordDict, temp, i + 1);
            }
        }
    }
}
```

解法二：使用原本word break 的解法先確定有解，再跑dfs以節省複雜度。

其程式碼如下：

```java
public class Solution {
    List<String> res = new ArrayList<String>();
    public List<String> wordBreak(String s, Set<String> wordDict) {
        if (s == null || wordDict == null || s.length() == 0 || wordDict.size() == 0) {
            return res;
        }
        
        int len = s.length();
        boolean[] canBreak = new boolean[s.length() + 1];
        Arrays.fill(canBreak, false);
        canBreak[0] = true;
        int max = getMaxLength(wordDict);
        
        //原本 word break的dp解法，用來判所是否有正確的解，如果有則跑dfs，否則直接返回空結果
        for (int i = 1; i <= s.length(); i++) {
            //取j=1與j<=i是因為java substring吃不到第i個char
            for (int j = 1; j <= i && j <= max; j++) {
                if (!canBreak[i - j]) {
                    continue;
                }
                String curString = s.substring(i - j, i);
                if (wordDict.contains(curString)) {
                     //有則break省時間
                     canBreak[i] = true;
                     break;
                }
            }
        }
        
        if (!canBreak[len]) {
            return res;
        }
        
        helper(s, wordDict, new StringBuilder(), 0);
        return res;
    }
    
    // 取出字典中最長word的長度，來節約複雜度。
    private int getMaxLength(Set<String> dict) {
        int max = 0;
        for(String word : dict) {
            max = Math.max(max, word.length());  
        }
        return max;
    }
    
    // 典型的dfs模版，找到適合的加入，返回時再刪掉該值。
    private void helper(String s, Set<String> wordDict, StringBuilder sb, int start) {
        if (start >= s.length()) {
            res.add(new String(sb));
            return;
        }
        
        for (int i = start + 1; i <= s.length(); i++) {
            String str = s.substring(start, i);
            if (wordDict.contains(str)) {
                int oldLen = sb.length(); // 紀錄oldlen晚點要刪掉該字串時可以用
                if (oldLen != 0) {
                    sb.append(" ");
                }
                sb.append(str);
                helper(s, wordDict, sb, i); // 注意是i
                sb.delete(oldLen, sb.length());
            }
        }
    }
}
```


updated 2015.11.24

之前的方法太長而且會超時，在此使用memory search來降低複雜度，程式碼如下：

```java
public class Solution {
    public List<String> wordBreak(String s, Set<String> wordDict) {
        return dfs(s, wordDict, new HashMap<String, LinkedList<String>>());
    }
    
    private List<String> dfs(String s, Set<String> wordDict, HashMap<String, LinkedList<String>> map) {
        if (map.containsKey(s)) {
            return map.get(s);
        }
        
        LinkedList<String> list = new LinkedList<String>();
        if (s.length() == 0) {
            list.add("");
            return list;
        }
        for (String word : wordDict) {
            if (s.startsWith(word)) {
                List<String> sublist = dfs(s.substring(word.length()), wordDict, map);
                for (String sub : sublist) {
                    list.add(word + (sub.isEmpty() ? "" : " ") + sub);
                }
            }
        }
        map.put(s, list);
        return list;
    }
}
```
---
###Reference
1. http://blog.csdn.net/linhuanmars/article/details/22452163
2. https://stupidcodergoodluck.wordpress.com/2013/11/16/leetcode-word-break-ii/ (解法二的來源)