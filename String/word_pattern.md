# Word Pattern

[Leetcode](https://leetcode.com/problems/word-pattern/)


```java
public class Solution {
    public boolean wordPattern(String pattern, String str) {
        if (str == null || str.length() == 0 || pattern == null || pattern.length() == 0) {
            return false;
        }
        
        HashMap<Character, String> map = new HashMap<Character, String>();
        HashSet<String> set = new HashSet<String>();
        String[] strs = str.split("\\s");
        if (pattern.length() != strs.length) {
            return false;
        }
        
        for (int i = 0; i < pattern.length(); i++) {
            char c = pattern.charAt(i);
            if (!map.containsKey(c)) {
                if(map.containsValue(strs[i])) {
                    return false;
                }
                map.put(c, strs[i]);
                set.add(strs[i]);
            } else {
                if (!map.get(c).equals(strs[i])) {
                    return false;
                } 
            }
        }
        return true;
    }
}
```