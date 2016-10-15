# Palindrome Permutation II

[Leetcode](https://leetcode.com/problems/palindrome-permutation-ii/)


題意：

Given a string s, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.

For example:

Given s = "aabb", return ["abba", "baab"].

Given s = "abc", return [].

解題思路：

updated 2015.12.4

網友神之解法


```java
public class Solution {
    public List<String> generatePalindromes(String s) {
        int odd = 0;
        String mid = "";
        List<String> res = new ArrayList<>();
        List<Character> list = new ArrayList<>();
        Map<Character, Integer> map = new HashMap<>();
        
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            map.put(c, map.containsKey(c) ? map.get(c) + 1 : 1);
            odd += map.get(c) % 2 != 0 ? 1 : -1;
        }
        
        // 奇數大於一個，無法作迴文
        if (odd > 1) {
            return res;
        }
        
        for (Map.Entry<Character, Integer> entry : map.entrySet()) {
            char key = entry.getKey();
            int val = entry.getValue();
            if (val % 2 != 0) {
                mid += key;
            }
            
            for (int i = 0; i < val / 2; i++) {
                list.add(key);
            }
        }
        
        getPerm(list, mid, new boolean[list.size()], new StringBuilder(), res);
        return res;
    }
    
    private void getPerm(List<Character> list, String mid, boolean[] used, StringBuilder sb, List<String> res) {
        if (sb.length() == list.size()) {
            res.add(sb.toString() + mid + sb.reverse().toString());
            sb.reverse();
            return;
        }
        
        for (int i = 0; i < list.size(); i++) {
            if (i > 0 && list.get(i) == list.get(i - 1) && !used[i - 1]) {
                continue;
            }
            
            if (!used[i]) {
                used[i] = true;
                sb.append(list.get(i));
                getPerm(list, mid, used, sb, res);
                used[i] = false;
                sb.setLength(sb.length() - 1);
            }
        }
    }
}
```

使用dfs去作，再搭配ispalindrome來判斷是否為迴文，但複雜度太高，會超時，程式碼如下：

```java
public class Solution {
    List<String> res;
    boolean[] isVisited;
    public List<String> generatePalindromes(String s) {
        res = new ArrayList<String>();
        isVisited = new boolean[s.length()];
        if (s == null || s.length() == 0) {
            return res;
        }
        
        helper(s, new StringBuilder());
        return res;
        
    }
    
    private void helper(String s, StringBuilder sb) {
        if (sb.length() == s.length()) {
            if (isPalindrome(sb.toString())) {
                res.add(sb.toString());
            }
        }
        
        for (int i = 0; i < s.length(); i++) {
            if (isVisited[i]) {
                continue;
            }
            isVisited[i] = true;
            sb.append(s.charAt(i));
            helper(s, sb);
            sb.setLength(sb.length() - 1);
            isVisited[i] = false;
        }
    }
    
    private boolean isPalindrome(String s) {
        int start = 0;
        int end = s.length() - 1;
        while (start <= end) {
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

---
###Reference
1. https://leetcode.com/discuss/53626/ac-java-solution-with-explanation