# Remove Duplicate Letters

[Leetcode](https://leetcode.com/problems/remove-duplicate-letters/)

題意：

Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

Example:

Given "bcabc"

Return "abc"

Given "cbacdcbc"

Return "acdb"

解題思路：

可以利用stack加上以升序的方式存放來幫助我們解這道題，因為題目要求是以字母順序排列的。

1. 首先建立一個count 陣列來計算出總共有多少字在裡面
2. 並且有一個inres 的陣列來紀錄第i個字元是否在結果裡。


接著跑遍整個字串，會有以下狀況：

1. 如果該字元的inRes[c]為真的話，表示該字元之前已存過了，所以跳過當前的字串
2. 若無的話，則不斷比較stack頂端的值，若比目前字元還大的話，則pop掉



```java
public class Solution {
    public String removeDuplicateLetters(String s) {
        if (s == null || s.length() < 2) {
            return s;
        }
        
        int len = s.length(); 
        int[] count = new int[128]; // 因為只有小寫字母，128足已
        boolean[] inRes = new boolean[128];
        char[] charArray = s.toCharArray();
        
        for (int i = 0; i < len; i++) {
            count[charArray[i]]++;
        }
        
        int end = -1;
        
        for (int i = 0; i < len; i++) {
            char c = charArray[i];
            
            // 如果已經存在的話則丟棄當前這個
            if (inRes[c] == true) {
                count[c]--;
                continue;
            }
            
            while (end >= 0 && charArray[end] >= c && count[charArray[end]] > 0) {
                char sc = charArray[end];
                end--;
                inRes[sc] = false;
            }
            
            charArray[++end] = c;
            count[c]--;
            inRes[c] = true;
        }
        
        return String.valueOf(charArray).substring(0, end + 1);
    }
}
```

---
###Reference
1. https://leetcode.com/discuss/74373/java-2ms-two-pointers-solution-or-stack-simulation-beats-72%25