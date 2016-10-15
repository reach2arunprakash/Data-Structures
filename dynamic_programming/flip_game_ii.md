# Flip Game II

[Leetcode](https://leetcode.com/problems/flip-game-ii/)


題意：

You are playing the following Flip Game with your friend: Given a string that contains only these two characters: + and -, you and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to determine if the starting player can guarantee a win.

For example, given s = "++++", return true. The starting player can guarantee a win by flipping the middle "++" to become "+--+".

Follow up:
Derive your algorithm's runtime complexity.


解題思路：

使用dfs的方式遞迴去作，一旦找到一個方式能讓對手輸的話，則返回true，否則返回false。

```java
public class Solution {
    public boolean canWin(String s) {
        if (s == null || s.length() < 2) {
            return false;
        }
        
        for (int i = 0; i < s.length() - 1; i++) {
            if (s.charAt(i) == '+' && s.charAt(i + 1) == '+') {
                String str = s.substring(0, i) + "--" + s.substring(i + 2);
                
                if (!canWin(str)) {
                    return true;
                }
            }
        }
        
        return false;
    }
}
```