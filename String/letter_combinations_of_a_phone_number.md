# Letter Combinations of a Phone Number

[]()

題意：

Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.

Cellphone

![](http://www.lintcode.com/media/problem/200px-Telephone-keypad2.svg.png)

Example

Given "23"

Return ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]

Note
Although the above answer is in lexicographical order, your answer could be in any order you want.

解題思路：

使用dfs，沒啥好說的，複雜度$$O(N^3)$$，程式碼如下：

```java
public class Solution {
    /**
     * @param digits A digital string
     * @return all posible letter combinations
     */
    String[] keypad = new String[] {" ", "", "abc", "def", "ghi", "jkl", "mno", 
                                    "pqrs", "tuv", "wxyz"};
    ArrayList<String> res = new ArrayList<String>();
    public ArrayList<String> letterCombinations(String digits) {
        
        if (digits == null || digits.length() == 0) {
            return res;
        }
        
        helper(0, new StringBuilder(), digits);
        return res;
    }
    
    private void helper(int pos, StringBuilder sb, String digits) {
        if (pos == digits.length()) {
            res.add(new String(sb.toString()));
            return;
        }
        
        String str = keypad[digits.charAt(pos) - '0'];
        for(int j = 0; j < str.length(); j++) {
            sb.append(str.charAt(j));
            helper(pos + 1, sb, digits);
            sb.setLength(sb.length() - 1);
        }

    }
}

```