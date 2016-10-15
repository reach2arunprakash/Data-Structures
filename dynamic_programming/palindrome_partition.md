#Palindrome Partition

[原題網址](http://www.lintcode.com/en/problem/palindrome-partitioning/)

解題思路：使用類似 combinations 的解法，但是要注意 substring 的用法。

>Substring：回傳一個新的字串，它是此字串的新的子字串。該子字串從指定的 beginIndex 處開始，一直到索引 endIndex - 1 處的字串。因此，該字串的長度為 endIndex-beginIndex。

```java
public class Solution {
    /**
     * @param s: A string
     * @return: A list of lists of string
     */
    List<List<String>> res;
    public List<List<String>> partition(String s) {
        
        res = new ArrayList<List<String>>();
        
        if (s == null || s.length() == 0) {
            res.add(new ArrayList<String>());
            return res;
        }
        
        helper(s, new ArrayList<String>(), 0);
        return res;
    }
    
    private void helper(String s, List<String> tempRes, int pos) {
        
        if (pos == s.length()) {
            res.add(new ArrayList<String>(tempRes));
            return;
        }
        
        for (int i = pos + 1; i <= s.length(); i++) {
            String prefix = s.substring(pos, i);
            if (!isPalindrome(prefix)) {
                continue;
            }
            tempRes.add(prefix);
            helper(s, tempRes, i);
            tempRes.remove(tempRes.size() - 1);
        }
    }
    
    private boolean isPalindrome(String s) {
        int start = 0;
        int end = s.length() - 1;
        
        while (start < end) {
            if (s.charAt(start) != s.charAt(end)) {
                return false;
            }
            start++;
            end--;
        }
        return true;
    }
}
```