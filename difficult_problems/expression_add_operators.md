# Expression Add Operators

[Leetcode](https://leetcode.com/problems/expression-add-operators/)

題意：

Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

Examples: 
```
"123", 6 -> ["1+2+3", "1*2*3"] 
"123", 15 -> ["12+3"]
"232", 8 -> ["2*3+2", "2+3*2"]
"105", 5 -> ["1*0+5","10-5"]
"00", 0 -> ["0+0", "0-0", "0*0"]
"3456237490", 9191 -> []
```

解題思路：

此為Combination Sum 的變形，我們可以使用dfs來幫助我們解這道題，加與減很好處理，這邊需要特別處理的是乘的部份

網友 [ethannnli](http://segmentfault.com/a/1190000003797204) 提供了以下詳細的思路：

1. 乘號之前是加號或減號，例如2+3*4，我們在2那裡算出來的結果，到3的時候會加上3，計算結果變為5。在到4的時候，因為4之前我們選擇的是乘號，這裡3就應該和4相乘，而不是和2相加，所以在計算結果時，要將5先減去剛才加的3得到2，然後再加上3乘以4，得到2+12=14，這樣14就是到4為止時的計算結果。

2. 另外一種情況是乘號之前也是乘號，如果2+3*4*5，這裡我們到4為止計算的結果是14了，然後我們到5的時候又是乘號，這時候我們要把剛才加的3*4給去掉，然後再加上3*4*5，也就是14-3*4+3*4*5=62。這樣5的計算結果就是62。
 
因為要解決上述幾種情況，我們需要這麼幾個變量，一個是記錄上次的計算結果currRes，一個是記錄上次被加或者被減的數prevNum，一個是當前準備處理的數currNum。當下一輪搜索是加減法時，prevNum就是簡單換成currNum，當下一輪搜索是乘法時，prevNum是prevNum乘以currNum。

>注意

>第一次搜索不添加運算符，只添加數字，就不會出現+1+2這種表達式了。
我們截出的數字不能包含0001這種前面有0的數字，但是一個0是可以的。這裡一旦截出的數字前導為0，就可以return了，因為說明前面就截的不對，從這之後都是開始為0的，後面也不可能了。

程式碼如下：

```java
public class Solution {
    List<String> res;
    public List<String> addOperators(String num, int target) {
        res = new ArrayList<String>();
        if (num == null || num.length() == 0) {
            return res;
        }
        helper(num, target, new String(), 0, 0);
        return res;
    }
    
    private void helper(String num, int target, String tempRes, long curRes, long prevNum) {
        if (curRes == target && num.length() == 0) {
            res.add(new String(tempRes));
            return;
        }
        
        for (int i = 1; i <= num.length(); i++) {
            String cur = num.substring(0, i);
            
            // 捨棄"00"之類的值
            if (cur.length() > 1 && cur.charAt(0) == '0') {
                return;
            }
            
            long curNum = Long.parseLong(cur);
            String next = num.substring(i); //把剛取出來的那個丟掉，因下一輪不能再取
            
            // 如果不是第一個數字的話，後面才要附上加減乘
            if (tempRes.length() != 0) {
                helper(next, target, tempRes + "+" + curNum, curRes + curNum, curNum);
                helper(next, target, tempRes + "-" + curNum, curRes - curNum, -curNum);
                helper(next, target, tempRes + "*" + curNum, (curRes - prevNum) + prevNum * curNum, prevNum * curNum);
            } else {
                helper(next, target, cur, curNum, curNum);
            }
        }
    }
}
```

---
###Reference
1. http://segmentfault.com/a/1190000003797204
2. 