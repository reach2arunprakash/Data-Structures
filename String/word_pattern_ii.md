# Word Pattern II

[Leetcode](https://leetcode.com/problems/word-pattern-ii/)


題意：

Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty substring in str.

**Examples:**
- pattern = "abab", str = "redblueredblue" should return true.
- pattern = "aaaa", str = "asdasdasdasd" should return true.
- pattern = "aabb", str = "xyzabcxzyabc" should return false.

**Notes:**

You may assume both pattern and str contains only lowercase letters.


解題思路：

使用dfs去作，


```java
public class Solution {
    Map<Character, String> map = new HashMap();
    Set<String> set = new HashSet();
    
    public boolean wordPatternMatch(String pattern, String str) {
        if (pattern == null || pattern.length() == 0) {
            return str == null || str.length() == 0;
        }    
        
        char key = pattern.charAt(0);
        
        if (map.containsKey(key)) {
            String value = map.get(key);
            
            // 若長度不對，或是不等於的話，直接返回false
            if (str.length() < value.length() || 
                !str.substring(0, value.length()).equals(value)) {
                    return false;
            }
            
            // 否則繼續往下比對
            if (wordPatternMatch(pattern.substring(1), str.substring(value.length()))) {
                return true;
            }
        } else {
            // 因不含該key，所以找出一個之前沒加過的substring當作value來加入到map中，
            // 並繼續往下比較。
            for (int i = 1; i <= str.length(); i++) {
                String substring = str.substring(0, i);
                if (set.contains(substring)) {
                    continue;
                }
                
                map.put(key, substring);
                set.add(substring);
                if (wordPatternMatch(pattern.substring(1), str.substring(i))) {
                    return true;
                }
                set.remove(substring);
                map.remove(key);
            }
        }
        
        return false;
    }
}
```


---
###Reference
1. https://leetcode.com/discuss/63393/20-lines-java-clean-solution-easy-to-understand