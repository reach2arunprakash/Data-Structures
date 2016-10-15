# Valid Parentheses

[Leetcode](https://leetcode.com/problems/valid-parentheses/)


題意：

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

Show Company Tags
Show Tags
Show Similar Problems


解題思路：


使用stack與hashmap來輔助我們作這道題，最後得檢查stack是否為空。


```java
public class Solution {
    public boolean isValid(String s) {
        HashMap<Character, Character> map = new HashMap<Character, Character>();
        map.put('(',')');
        map.put('{','}');
        map.put('[',']');
        
        Stack<Character> st = new Stack<Character>();
        for (int i = 0; i < s.length(); i++) {
            char cur = s.charAt(i);
            if (map.containsKey(cur)) {
                st.push(cur);
            } else {
                if (st.empty() || map.get(st.pop()) != cur) {
                    return false;
                }
            }
        }
        
        return st.empty();
    }
}
```