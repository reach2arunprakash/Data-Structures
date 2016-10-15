#Roman to Integer

[原題連結](http://www.lintcode.com/en/problem/roman-to-integer/)

題意：

Given a roman numeral, convert it to an integer.

The answer is guaranteed to be within the range from 1 to 3999.

Have you met this question in a real interview? Yes
Example
IV -> 4

XII -> 12

XXI -> 21

XCIX -> 99

Clarification
What is Roman Numeral?

https://en.wikipedia.org/wiki/Roman_numerals
https://zh.wikipedia.org/wiki/%E7%BD%97%E9%A9%AC%E6%95%B0%E5%AD%97
http://baike.baidu.com/view/42061.htm

解題思路：羅馬數字的規則即左減右加，一但當前數並上一個數還大，代表是減，因此除了不斷累加當前的數，一但遇到需要減的數，，減掉兩倍該數，因該數在上一步驟已加上了，需減掉。

```java
public int romanToInt(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    
    HashMap<Character, Integer> map = new HashMap<Character, Integer>();
    map.put('I', 1);
    map.put('V', 5);
    map.put('X', 10);
    map.put('L', 50);
    map.put('C', 100);
    map.put('D', 500);
    map.put('M', 1000);
    
    int result = 0;
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        result += map.get(c);
        if (i != 0) {
            char prev = s.charAt(i-1);
            if (map.get(c) > map.get(prev)) {
                result -= map.get(prev) * 2;
            }
        }
    }
    return result;
}
```
>Time Complexity..$$O(N)$$
