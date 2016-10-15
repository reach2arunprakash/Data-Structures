#Count and Say

[原題網址](http://www.lintcode.com/en/problem/count-and-say/)

題意：

The count-and-say sequence is the sequence of integers beginning as follows:

```1, 11, 21, 1211, 111221, ...```

1 is read off as "one 1" or 11.

11 is read off as "two 1s" or 21.

21 is read off as "one 2, then one 1" or 1211.

Given an integer n, generate the nth sequence.

解題思路：

一開始題目看不太懂，其實就是第 i 個字串是把第 i-1 個字串數完個數並念出來，

如 i = 2 時，因seq[0] = "1"，就是一個1，則 seq[1] = "11"，

當 i = 3 時，因seq[1] = "11"，就是兩個1，則 seq[2] = "21"，

程式碼如下：


```java
public String countAndSay(int n) {
    
    String prev = "1";
    
    for (int i = 2; i <= n; i++) {
        StringBuilder cur = new StringBuilder();
        int count = 1;
        for (int j = 1; j < prev.length(); j++) {
            if (prev.charAt(j - 1) == prev.charAt(j)) {
                count++;
            } else {
                cur.append(count).append(prev.charAt(j - 1));
                count = 1;
            }
        }
        
        // 為了處理最後一位，加上若 prev的長度只有一位的話。
        cur.append(count).append(prev.charAt(prev.length() - 1));
        prev = cur.toString();
    }
    
    return prev;
}
```
---
###Reference
1. http://fisherlei.blogspot.com/2013/01/leetcode-count-and-say-solution.html
2. http://jane4532.blogspot.com/2013/10/count-and-sayleetcode.html