# Longest Valid Parentheses

[Leetcode](https://leetcode.com/problems/longest-valid-parentheses/)


題意： 給定一個字串只包含括號，找出最長的合法括號


解題思路：

updated on 2015.12.21

使用dp來解，網友[Lexi](https://leetcodenotes.wordpress.com/2013/10/19/leetcode-longest-valid-parentheses-%E8%BF%99%E7%A7%8D%E6%8B%AC%E5%8F%B7%E7%BB%84%E5%90%88%EF%BC%8C%E6%9C%80%E9%95%BF%E7%9A%84valid%E6%8B%AC%E5%8F%B7%E7%BB%84%E5%90%88%E6%9C%89%E5%A4%9A/) 提供以下思路：

- d[i]: 以i開頭的最長valid parentheses有多長。

- d[i] =
    - if  (str[i] == ")")，以右括號開頭必定invalid，d[i] = 0
    
    - if (str[i] == "(")，以左括號開頭

        - 我們想看對應的有沒有右括號。因為d[i + 1]代表的sequence肯定是左括號開頭右括號結尾，所以我們想catch((…))這種情況。j = i + 1 + d[i + 1]，正好就是str[i]後面越過完整sequence的下一個，若是右括號，d[i] = 2 + d[i + 1]
        
        - 除此之外，還有包起來後因為把後面的右括號用了而導致跟再往後也連起來了的情況，如((..))()()()。所以d[i]還要再加上j後面那一段的d[j + 1]


```java
public class Solution {
    public int longestValidParentheses(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int max = 0;
        int len = s.length();
        int[] dp = new int[len];
        
        // dp[i] means substring starts with i has max valid length of dp[i]
        dp[len - 1] = 0;
        
        for (int i = s.length() - 2; i >= 0; i--) {
            if (s.charAt(i) == ')') {
                dp[i] = 0;
            } else {
                int j = (i + 1) + dp[i + 1];
                if (j < len && s.charAt(j) == ')') {
                    // (()()) 的外包情況
                    dp[i] = dp[i + 1] + 2;
                    if (j + 1 < s.length()) {
                        //()() 的後面還有的情況
                        dp[i] += dp[j + 1];
                    }
                    
                }
            }
            max = Math.max(max, dp[i]);
        }
        
        return max;
    }
}
```

暴力解，把任何組合全丟進去isvalid的方法來判斷，時間複雜度約O(N^3)

程式碼如下，在leetcode會超時：

```java
public class Solution {
    public int longestValidParentheses(String s) {
        int maxLength = 0;
        if (s == null || s.length() == 0){
            return maxLength;
        }
        int j = 0;
        for (int i = 0; i < s.length() && j <= s.length(); i++) {
            String substring = s.substring(i, j);
            while(isValid(substring)) {
                maxLength = Math.max(maxLength, j - i + 1);
                j++;
                substring = s.substring(i, j);
            }
        }
        
        return maxLength;
    }
    
    private boolean isValid(String s) {
        
        if (s == null || s.length() == 0) {
            return true;
        }
        
        char[] charArray = s.toCharArray();
        Stack<Character> stack = new Stack<Character>();
        for (int i = 0; i < charArray.length; i++) {
            if (charArray[i] == ')') {
                if (stack.isEmpty() || stack.peek() != ')') {
                    return false;
                }
                stack.pop();
            } else {
                stack.push(charArray[i]);
            }
        }
        return stack.isEmpty();
    }
}
```

另外可用 stack 法，stack中存放尚未配對的括號，只要一但找到能配對的括號，則pop掉，並算長度，否則就push進去。

網友[Lexi](https://leetcodenotes.wordpress.com/2013/10/19/leetcode-longest-valid-parentheses-%E8%BF%99%E7%A7%8D%E6%8B%AC%E5%8F%B7%E7%BB%84%E5%90%88%EF%BC%8C%E6%9C%80%E9%95%BF%E7%9A%84valid%E6%8B%AC%E5%8F%B7%E7%BB%84%E5%90%88%E6%9C%89%E5%A4%9A/) 提供以下思路：

1. stack裡面裝的一直是「還沒配好對的那些可憐的括號的index」

2. 是'('的時候push

3. 是')'的時候，說明可能配對了；看stack top是不是左括號，不是的話，push當前右括號

4. 是的話，pop那個配對的左括號，然後update res：i和top的（最後一個配不成對的）index相減，就是i屬於的這一段的當前最長。如果一pop就整個棧空了，說明前面全配好對了，那res就是最大=i+1

程式碼如下：

```java
public class Solution {
    public int longestValidParentheses(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        Stack<Integer> stack = new Stack<Integer>();
        int res = 0;
        for (int i = 0; i < s.length(); i++) {
            if (!stack.isEmpty() && s.charAt(i) == ')' && s.charAt(stack.peek()) == '(') {
                stack.pop();
                if (stack.isEmpty()) {
                    res = i + 1;
                } else {
                    res = Math.max(res, i - stack.peek());
                }
            } else {
                stack.push(i);
            }
        }
        
        return res;
    }
}
```

---
###Reference
1. https://leetcodenotes.wordpress.com/2013/10/19/leetcode-longest-valid-parentheses/