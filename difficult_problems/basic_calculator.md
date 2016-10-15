# Basic Calculator

[Leetcode](https://leetcode.com/problems/basic-calculator/)

題意：

mplement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

You may assume that the given expression is always valid.

Some examples:
```
"1 + 1" = 2
" 2-1 + 2 " = 3
"(1+(4+5+2)-3)+(6+8)" = 23
```
Note: Do not use the eval built-in library function.

題意：

遞迴法：遇到"("則進入下一個遞迴，遇到")"則返回遞迴，以下為網友提供的解法，巧妙的運用StringTokenizer來分split各個元素。

以下為StringTokenizer的用法，取自官網

```
public StringTokenizer(String str,
               String delim,
               boolean returnDelims) <-- 重點，此設為true時，表示delims除了用來分解字串外，也會被當成token

Parameters:
str - a string to be parsed.
delim - the delimiters.
returnDelims - flag indicating whether to return the delimiters as tokens.
```

```java
public class Solution {
    public int calculate(String s) {
		return calc(new StringTokenizer(s, " ()+-",  true));
	}

 	int calc(StringTokenizer st) {
 		int sofar = 0;
 		boolean plus = true; // 最後看到的是什麼符號，因只有加與減所以用boolean即可

 		while(st.hasMoreTokens()) {
 			int val = 0;
 			String next = st.nextToken();
 			switch (next) {
 				case "(":
 					val = calc(st);
 					sofar += (plus ? val : -val);
 					break;
 				case ")":
					return sofar;
				case "+":
					plus = true;
					break;
				case "-":
					plus = false;
					break;
				case " ":
					break;
				default :
					val = Integer.parseInt(next);
					sofar += (plus ? val : - val);
					break;
 			}
 		}
 		return sofar;
 	}
}
```

法二：stack，網友提供了神之解法，非常簡潔，stack中只存放了+1與-1，一旦遇到(，則把當前的sign放進stack中，接著()裡面的值全部用stack最頂端的sign來作變化。因為只有+- 所以不用考慮前後優先權的問題，其程式碼如下：

```java
public class Solution {
    public int calculate(String s) {
        int res = 0;
        int val = 0;
        int sign = 1;
        Stack<Integer> st = new Stack<Integer>();
        s = s.trim(); // remove leading and trailing spaces
        
        for (int i = 0; i < s.length(); i++) {
            char cur = s.charAt(i);
            if (cur == '(') {
                st.push(sign);
            } else if (cur == ')') {
                st.pop();
            } else if (cur == '+' || cur == '-') {
                // 一旦遇到ops時，得先把之前的處理完，再更新sign
                // 並把val重設為0，準備讀下一個val
                res += sign * val;
                val = 0;
                if (!st.isEmpty()) {
                    sign = cur == '-' ? st.peek()*(-1) : st.peek();
                } else {
                    sign = cur == '-' ? -1 : 1;
                }
            } else if (cur != ' ') {
                val = val * 10 + (cur - '0');
            }
        }
        
        res += val * sign;
        return res;
        
    }
}
```

---
###Reference
1.https://leetcode.com/discuss/55550/java-solution-use-stack