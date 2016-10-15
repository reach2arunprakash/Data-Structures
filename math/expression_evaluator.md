# Expression Evaluator

[]()


```java
public class Solution {
    public int calculate(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        Stack<Integer> valS = new Stack<Integer>();
        Stack<Character> opS = new Stack<Character>();
        char[] charArray = s.toCharArray();
        
        for (int i = 0; i < s.length(); i++) {
            char cur = charArray[i];
            if (cur == ' ') {
                continue;
            }
            
            if (cur >= '0' && cur <= '9') {
                StringBuffer sb = new StringBuffer();
                while(i < s.length() && charArray[i] >= '0' && charArray[i] <= '9') {
                    sb.append(charArray[i]);
                    i++;
                }
                valS.push(Integer.parseInt(sb.toString()));
            } else if (cur == '(') {
                opS.push(cur);
            } else if (cur == ')') {
                while (opS.peek() != '(') {
                    valS.push(applyOp(opS.pop(), valS.pop(), valS.pop()));
                }
                opS.pop(); // pop ( 
            } else if (cur == '+' || cur == '-' || cur == '*' || cur == '/') {
                // 此時要判斷operator的優先級
                // 如果當前的operator比stack頂端的元素優先級還高的話，則直接push
                // 否則要把當前的operator，push進stack中
                while(!opS.isEmpty() && checkPriority(cur, opS.peek())) {
                    valS.push(applyOp(opS.pop(), valS.pop(), valS.pop()));
                }
                opS.push(cur);
            }
        }
        // 最後stack中還有沒算完的要處理完
        while (!opS.isEmpty()) {
            valS.push(applyOp(opS.pop(), valS.pop(), valS.pop()));
        }
        return valS.pop();
    }
    
    private int applyOp(char op, int valOne, int valTwo) {
        if (op == '+') {
            return valTwo + valOne;
        } else if (op == '-') {
            return valTwo - valOne;
        } else if (op == '*') {
            return valTwo * valOne;
        } else {
            return valTwo / valOne;
        }
    }
    
    private boolean checkPriority(char c1, char c2) {
        if (c1 == '(' || c2 == ')') {
            return false;
        }
        if (((c1 == '*') || (c1 == '/')) && ((c2 == '+') || (c2 == '-'))) {
            return false;
        } else {
            return true;
        }
    }
}
```

---
###Reference
1. http://www.geeksforgeeks.org/expression-evaluation/