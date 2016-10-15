#Valid Number

[Lintcode](http://www.lintcode.com/en/problem/valid-number/)

題意：

Validate if a given string is numeric.

Example

"0" => true

" 0.1 " => true

"abc" => false

"1 a" => false

"2e10" => true

解題思路：這題困難在於太多 corner case 要考慮，首先我們把字串由左到右分成以下四個部份：

1. 前綴空白 (可有可不有)
2. 正負號 (可有可不由)
3. 數字 (必要)
4. 後綴空白 (可有可不有)

其實我們只需要檢查第 3 個部份來驗證是否為有效數字。

接著我們需要把數字的部份切成以下部份來分析：

1. 整數
2. 小數
3. 小數點

解題步驟如下：

1. 忽略正負號與前綴空白。
2. 檢查每一個數字是否為正整數，若是則繼續，否則結束。
3. 若有小數點，則繼續往下檢查正整數。
4. 把後面的後綴空白一一省略
5. 最後檢查isvalid是否為真，以及 i是否已經到字串結尾。


程式碼如下，目前並未考慮科學符號：

```java
public class Solution {
    /**
     * @param s the string that represents a number
     * @return whether the string is a valid number
     */
    public boolean isNumber(String s) {
        
        int i = 0;
        int len = s.length();
        boolean isValid = false;
        
        // 忽略正負號與 leading whitespaces
        while (i < len && (s.charAt(i) == '-' || s.charAt(i) == '+' || Character.isWhitespace(s.charAt(i)))) {
            i++;
        }
        
        // 一一檢查每個digit是否為數字
        while (i < len && Character.isDigit(s.charAt(i))) {
            i++;
            isValid = true;
        }
        
        // 若含小數的話，則繼續往下檢查
        if (i < len && s.charAt(i) == '.') {
            i++;
            while (i < len && Character.isDigit(s.charAt(i))) {
                i++;
                isValid = true; // 防止有這種 .1343 的情況
            }
        }
        
        // 忽略 trailing spaces
        while (i < len && Character.isWhitespace(s.charAt(i))) {
            i++;
        }
        
        //除了檢查isvalid外，記得注意i是否到底了。
        return isValid && i == len;
        
    }
}
```

若考慮科學符號 $$e$$ 的話，則需要再多加一段code，在小數點檢查之後。

```java
if (isNumeric && i < n && s.charAt(i) == 'e') {
   i++;
   isNumeric = false;
   if (i < n && (s.charAt(i) == '+' || s.charAt(i) == '-')) i++;
   while (i < n && Character.isDigit(s.charAt(i))) {
i++;
      isNumeric = true;
   }
}
```