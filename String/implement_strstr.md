# Implement strStr()

[Leetcode](https://leetcode.com/problems/implement-strstr/)

題意：

Implement strStr().

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

解題思路：

brute force，注意，一但needle為空時，直接返回0而非-1。

```java
public class Solution {
    public int strStr(String haystack, String needle) {
        if (haystack == null || needle == null) {
            return -1;
        }
        if (needle.length() == 0) {
            return 0;
        }
        
        int hLen = haystack.length();
        int nLen = needle.length();
        for (int i = 0; i < hLen - nLen + 1; i++) {
            int j;
            for (j = 0; j < nLen; j++) {
                if (haystack.charAt(i + j) != needle.charAt(j)) {
                    break;
                }
            }
            if (j == nLen) {
                return i;
            }
        }
        return -1;
    }
}
```

kmp 有機會要理解