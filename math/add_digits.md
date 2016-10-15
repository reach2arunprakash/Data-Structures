#Add Digits

[原題連結](https://leetcode.com/problems/add-digits/)

題意：

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

For example:

Given ```num = 38```, the process is like: ```3 + 8 = 11, 1 + 1 = 2```. Since 2 has only one digit, return it.

Follow up:
Could you do it without any loop/recursion in $$O(1)$$ runtime?

解法一(循環)，程式碼如下：

```java
public int addDigits(int num) {
    
    if (num < 10) {
        return num;
    }
    
    while (num >= 10) {
        int temp = num;
        int result = 0;
        while (temp > 0) {
            result += temp % 10;
            temp /= 10;
        }
        
        num = result;
    }
    
    return num;
}
```
> Time Complexity：待分解

---
網友 [書影](http://bookshadow.com/weblog/2015/08/16/leetcode-add-digits/) 提供以下觀查法

方法II：觀察法

根據提示，由於結果只有一位數，因此其可能的數字為0 - 9

使用方法I的代碼循環輸出0 - 19的運行結果：
```
in  out  in  out
0   0    10  1
1   1    11  2
2   2    12  3
3   3    13  4
4   4    14  5
5   5    15  6
6   6    16  7
7   7    17  8
8   8    18  9
9   9    19  1
```
可以發現輸出與輸入的關係為：

>out = (in - 1) % 9 + 1


```java
public int addDigits2(int num) {
    
    if (num < 10) {
        return num;
    }
    
    return (num % 9 != 0) ? num % 9 : 9;
}
```

> Time Complexity：$$O(1)$$

---
###Reference
1. http://bookshadow.com/weblog/2015/08/16/leetcode-add-digits/