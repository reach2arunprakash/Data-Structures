#Minimum Window Substring

[原題網址](http://www.lintcode.com/en/problem/minimum-window-substring/)

題意：給定source 與target兩個字串，找出source最短的子字串包含target所有的字串。

解題思路：使用兩個陣列分別記錄 source 與 target 的字符個數，其中 source 只紀錄 i 到 j 之間的字符個數。

```java
public class Solution {
    /**
     * @param source: A string
     * @param target: A string
     * @return: A string denote the minimum window
     *          Return "" if there is no such a string
     */
    public String minWindow(String source, String target) {
        
        if (source == null || source.length() == 0 || target == null || target.length() == 0) {
            return "";
        }
        

        int[] sourceHash = new int[256];
        int[] targetHash = new int[256];
        
        for (int i = 0; i < target.length(); i++) {
            targetHash[target.charAt(i)] += 1;
        }
        
        int length = Integer.MAX_VALUE;
        int[] idx = new int[2];
        idx[0] = -1;
        idx[1] = -1;
        
        for (int i = 0, j = 0; i < source.length(); i++) {
            while (j < source.length() && !valid(sourceHash, targetHash)) {
                sourceHash[source.charAt(j)] += 1;
                j++;
            }
            if (valid(sourceHash, targetHash)) {
                if (length > j - i) {
                    idx[0] = i;
                    idx[1] = j;
                    length = j - i;
                }
            }
            sourceHash[source.charAt(i)] -= 1;
        }
        
        if (idx[0] == -1 && idx[1] == -1) {
            return "";
        } else {
            return source.substring(idx[0], idx[1]);
        }
        
    }
    
    private boolean valid(int[] sourceHash, int[] targetHash) {
        
        for (int i = 0; i < targetHash.length; i++) {
            if (targetHash[i] > 0 && sourceHash[i] < targetHash[i]) {
                return false;
            }
        }
        
        return true;
    }
}

```

Updated by 2015.10.30，思路類似，不同的事此時j當作substring的頭，i當尾。
 

```java
public class Solution {
    public String minWindow(String s, String t) {
        if (s == null || t == null) {
            return "";
        }
        
        // 假設為ascii編碼，只需要O(1)的空間來當hash table
        // 先把所有target字串中的每個字元個數紀錄在tHash中。
        int[] sHash = new int[256];
        int[] tHash = new int[256];
        Arrays.fill(sHash, 0);
        Arrays.fill(tHash, 0);
        int tCount = t.length();
        for (int i = 0; i < t.length(); i++) {
            tHash[t.charAt(i)]++;
        }
        
        // i 代表子字串的尾
        // j 代表子字串的頭
        int j = 0;
        int sCount = 0;
        int minLength = Integer.MAX_VALUE;
        String minStr = "";
        for (int i = 0; i < s.length(); i++) {
            if (tHash[s.charAt(i)] > 0) {
                sCount++;
            }
            tHash[s.charAt(i)]--;
            
            // 表示已包含所有 target的字元，並
            while(sCount >= tCount) {
                if (minLength > i - j + 1) {
                    minLength = i - j + 1;
                    minStr = s.substring(j , i + 1);
                }
                // 準備要把s字串的第j個字符刪掉並把j往前移了，因此把target該字元的數加回來
                tHash[s.charAt(j)]++;
                if (tHash[s.charAt(j)] > 0) {
                    sCount--;
                }
                j++;
            }
        }
        
        return minStr;
    }
}
```

---
###Reference
1. http://www.jiuzhang.com/solutions/minimum-window-substring/