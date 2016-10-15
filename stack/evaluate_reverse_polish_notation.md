#Evaluate Reverse Polish Notation

[原題連結](http://www.lintcode.com/en/problem/evaluate-reverse-polish-notation/)

```java
public class Solution {
    /**
     * @param tokens The Reverse Polish Notation
     * @return the value
     */
    public int evalRPN(String[] tokens) {
        
        if (tokens == null || tokens.length == 0) {
            return 0;
        }
        
        Stack<Integer> stack = new Stack<Integer>();
        String operators = "+-*/";
        
        for (int i = 0; i < tokens.length; i++) {
            
            String cur = tokens[i];
            
            if (operators.contains(cur)) {
                int b = stack.pop();
                int a = stack.pop();
                stack.push(calculate(a, b, cur.charAt(0)));
            } else {
                int value = Integer.parseInt(tokens[i]);
                stack.push(value);
            }
        }
        
        return stack.pop();
    }
    
    private int calculate(int a, int b, char operator) {
        
        switch(operator) {
            case '+':
                return a + b;
            case '-':
                return a - b;
            case '*':
                return a * b;
            case '/':
                return a / b;
        }
        return 0;
    }
}

```