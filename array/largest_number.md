#Largest Number

[Lintcode]()

題意：

Given a list of non negative integers, arrange them such that they form the largest number.

**Example**

Given [1, 20, 23, 4, 8], the largest formed number is 8423201.

**Note**

The result may be very large, so you need to return a string instead of an integer.

解題思路：

網友 [cherylintcode](http://cherylintcode.blogspot.com/2015/07/largest-number.html) 提供以下思路：

1. 先將所有數字轉成字串
2. 使用特製的 comparator，將兩個字串的組合作排序，如比較 23 與 4 ，他們有兩種組合 lr(423) 與 rl(234)，接著再透過 字串的 compareTo 函數來比較兩者的 ascii 值，較小的在前，較大在後。
3. 字串陣列排序完後，從後往前不斷的將元素 append 到 string builder上。
4. 返回答案即可。

> 為了防止 [0, 0] 的情況，記得在最後輸出前，檢查第一位是否為0 ，若是則返回0 ，否則返回 res。

程式碼如下：

```java
public class Solution {
    /**
     *@param num: A list of non negative integers
     *@return: A string
     */
    public String largestNumber(int[] num) {
        
        if (num == null || num.length == 0) {
            return "";
        }
        
        int len = num.length;
        String[] numString = new String[len];
        
        for (int i = 0; i < len; i++) {
            numString[i] = String.valueOf(num[i]);
        }
        
        Arrays.sort(numString, new stringComparator());
        
        // 因是由小到大，我們需要從後面開始append
        StringBuilder sb = new StringBuilder();
        for (int i = len - 1; i >= 0; i--) {
            sb.append(numString[i]);
        }
        
        
        // 防止 [0, 0] 這種情況
        if (sb.charAt(0) == '0') {
            return "0";
        }
        
        return sb.toString();
    }
    
    
    // 拿兩個值的組合來作排序，看哪個較小就排前面。
    class stringComparator implements Comparator<String> {
        public int compare(String s1, String s2) {
            String lr = s1 + s2;
            String rl = s2 + s1;
            return lr.compareTo(rl);
        }
    }
}
```





---
###Reference
1. http://cherylintcode.blogspot.com/2015/07/largest-number.html