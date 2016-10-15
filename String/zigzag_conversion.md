#ZigZag Conversion

[原題網址](https://leetcode.com/problems/zigzag-conversion/)

題意：

The string **"PAYPALISHIRING"** is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```plain
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: **"PAHNAPLSIIGYIR"**
Write the code that will take a string and make this conversion given a number of rows:

**convert("PAYPALISHIRING", 3)** should return **"PAHNAPLSIIGYIR"**.

解題思路：

把字轉換成index來看就快多了，假設 n = 3

``` plain
P   A
A P L
Y   I
```
```
0   4
1 3 5
2   6
```
以列來看(先不看斜的)，則每一列的前後兩個值的index便差了 2*n - 2，而斜的值便是 j + n - 2 * i，j 代表 column ，i 代表 row 。

原始碼如下：

```java
public String convert(String s, int numRows) {
        
    if (numRows <= 1) {
        return s;
    }
    
    StringBuilder str = new StringBuilder();
    int size = numRows * 2 - 2;
    
    for (int i = 0; i < numRows; i++) {
        for (int j = i; j < s.length(); j += size) {
            str.append(s.charAt(j));
            if (i > 0 && i < numRows - 1) {
                // 重點怎麼處理斜的部份
                int temp = j + size - 2 * i;
                if (temp < s.length()) {
                    str.append(s.charAt(temp));
                }
            }
        }
    }
    
    return str.toString();
}
```
---
###Reference
1. http://www.cnblogs.com/springfor/p/3893499.html#3209156