# Group Anagrams

[Leetcode](https://leetcode.com/problems/anagrams/)

題意：


解題思路：

使用hashmap，key為排序後的字串，value為所有擁有相同排序結果的字串們。

程式碼如下，時間複雜度為 $$(N*KlogK)$$：

```java
public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> map = new HashMap<String, List<String>>();
        List<List<String>> res = new ArrayList<List<String>>();
        Arrays.sort(strs); //很重要，因test case會比較這個順序
        
        if (strs == null || strs.length == 0) {
            return res;
        }
        
        // key則為每個字串排序後的值，value則為相同值的字串們
        for (int i = 0; i < strs.length; i++) {
            char[] charArray = strs[i].toCharArray();
            Arrays.sort(charArray); // 把字串排序後轉為key
            String key = new String(charArray);
            
            if (map.containsKey(key)) {
                map.get(key).add(strs[i]);
            } else {
                map.put(key, new ArrayList<String>());
                map.get(key).add(strs[i]);
            }
        }
        
        // 把所有value都放進res即可
        for (List<String> list : map.values()) {
            res.add(list);
        }
        
        return res;
    }
}
```