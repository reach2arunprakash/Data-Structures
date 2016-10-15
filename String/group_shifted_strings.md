# Group Shifted Strings

[Leetcode](https://leetcode.com/problems/group-shifted-strings/)

**題意：**

Given a string, we can "shift" each of its letter to its successive letter, for example: "abc" -> "bcd". We can keep "shifting" which forms the sequence:
```
"abc" -> "bcd" -> ... -> "xyz"
```
Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

For example, given: ```["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"]```, 
Return:
```
[
  ["abc","bcd","xyz"],
  ["az","ba"],
  ["acef"],
  ["a","z"]
]
```
Note: For the return value, each inner list's elements must follow the lexicographic order.


**解題思路：**


```java
public class Solution {
    public List<List<String>> groupStrings(String[] strings) {
        List<List<String>> res = new ArrayList<List<String>>();
        if (strings == null || strings.length == 0) {
            return res;
        }
        
        HashMap<String, List<String>> map = new HashMap<String, List<String>>();
        
        for (String str : strings) {
            int offset = str.charAt(0) - 'a';
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < str.length(); i++) {
                char c = (char) (str.charAt(i) - offset);
                if (c < 'a') {
                    c += 26;
                }
                sb.append(c);
            }
            String key = sb.toString();
            if (!map.containsKey(key)) {
                map.put(key, new ArrayList<String>());
            }
            map.get(key).add(str);
        }
        
        for (String key : map.keySet()) {
            List<String> list = map.get(key);
            Collections.sort(list);
            res.add(list);
        }
        
        return res;
        
    }
}
```