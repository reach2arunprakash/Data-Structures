#Anagrams

[原題網址](http://www.lintcode.com/en/problem/anagrams/)

解題思路：利用 [Compare String](draft/string/compare_strings.md) 中的程式來算兩者是否為 Anagram ，去跟陣列中的每個字串作比較，為了不作太多不必要的運算，在此使用了 hasAnagram 陣列，只要之有找到可搭配的 anagram 則把該位置設為真，一但遇到為真，則把該字串加入結果裡。

>有一個特點：單詞裡的字母的種類和數目沒有改變，只是改變了字母的排列順序，由此我們可以想到，只要將幾個單詞按照字母順序進行排序，就可以通過比較判斷他們是否是anagrams。

```java
public class Solution {
    /**
     * @param strs: A list of strings
     * @return: A list of strings
     */
    public List<String> anagrams(String[] strs) {
        
        List<String> res = new ArrayList<String>();
        boolean[] hasAnagram = new boolean[strs.length];
        
        if (strs.length == 0) {
            return res;
        }
        
        for (int i = 0; i < strs.length; i++) {
            if (hasAnagram[i]) {
                res.add(strs[i]);
                continue;
            }
            for (int j = i + 1; j < strs.length; j++) {
                if (isAnagram(strs[i], strs[j])) {
                    hasAnagram[i] = true;
                    hasAnagram[j] = true;
                    if (!res.contains(strs[i])) {
                        res.add(strs[i]);
                    }
                }
            }
        }
        
        return res;
    }
    
    private boolean isAnagram(String s, String t) {
        
        if (s.length() != t.length()) {
            return false;
        }
        
        int[] map = new int[256];
        for (int i = 0; i < s.length(); i++) {
            map[s.charAt(i) - 'a']++;
            map[t.charAt(i) - 'a']--;
        }
        
        for (int i = 0; i < 256; i++) {
            if (map[i] != 0) {
                return false;
            }
        }
        
        return true;
    }
}

```
>Time Complexity：$$O(N^3)$$