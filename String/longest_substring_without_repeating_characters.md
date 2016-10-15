#Longest Substring Without Repeating Characters

[原題網址](http://www.lintcode.com/en/problem/longest-substring-without-repeating-characters/)

題意：給定一個字串，找出最長的子字串其中不含重複字元

解題思路：與 [Minimum Size Subarray Sum](array/minimum_size_subarray_sum.md)一樣使用兩根指針的模板。假設字符為 ASCII 編碼，因此利用一個256大小的陣列來紀錄當前字符的出現次數即可。
原本使用了 hashmap ，但是維護 hashmap 需要花大量的時間，會超時。

```java
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    
    int length = 0;
    int[] map = new int[256];
    
    for (int i = 0, j = 0; i < s.length(); i++) {
        
        while (j < s.length() && map[s.charAt(j)] == 0) {
            map[s.charAt(j)] += 1;
            j++;
        }
        
        length = Math.max(length, j - i);
        
        map[s.charAt(i)] -= 1;
    }
    
    return length;
}
```

>Time Complexity：$$O(N)$$