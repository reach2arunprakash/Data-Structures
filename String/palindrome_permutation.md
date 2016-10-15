# Palindrome Permutation

[Leetcode](https://leetcode.com/problems/palindrome-permutation/)

題意：

Given a string, determine if a permutation of the string could form a palindrome.

For example,
"code" -> False, "aab" -> True, "carerac" -> True.

給定一個字串，判所是否可以排成迴文


解題思路：

update on 2015.12.4

```java
public class Solution {
    public boolean canPermutePalindrome(String s) {
        int[] map = new int[256];
        for (int i = 0; i < s.length(); i++) {
            map[s.charAt(i)]++;
        }
        
        boolean oneBefore = false;
        for (int i = 0; i < 256; i++) {
            if (oneBefore && (map[i] % 2 == 1)) {
                return false;
            } else if (!oneBefore && (map[i] % 2 == 1)) {
                oneBefore = true;
            }
        }
        return true;
    }
}
```

使用一個hash map分別紀錄每個character的個數，接著根據以下情況來判斷

1. 如果字串長度為偶數，則所有字元的字數皆為偶數
2. 如果字串長度為奇數，則只能有一個字元的字數為奇數，其他皆為偶數


程式碼如下：

```java
public class Solution {
    public boolean canPermutePalindrome(String s) {
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        for (int i = 0; i < s.length(); i++) {
            char cur = s.charAt(i);
            if (!map.containsKey(cur)) {
                map.put(cur, 0);
            }
            map.put(cur, map.get(cur) + 1);
        }
        
        if (s.length() % 2 == 0) {
            for (int value : map.values()) {
                if (value % 2 == 1) {
                    return false;
                }
            }
            return true;
        } else {
            boolean one = false;
            for (int value : map.values()) {
                if (value % 2 == 1) {
                    if (!one) {
                        one = true;
                    } else {
                        return false;
                    }
                }
            }
            return true;
        }
    }
}
```

網友提供更精簡的作法，利用一個set，每個char在當前set中最多出現一次，如果沒在set中，則加入，在set中，則把set中的char移除，最後判斷set的size是否為0 或1。

```java
public class Solution {
    public boolean canPermutePalindrome(String s) {
        if (s == null || s.length() < 2) {
            return true;
        }
        
        HashSet<Character> set = new HashSet<Character>();
        for (int i = 0; i < s.length(); i++) {
            char cur = s.charAt(i);
            if (!set.contains(cur)){
                set.add(cur);
            } else {
                set.remove(cur);
            }
        }
        
        return set.size() == 0 || set.size() == 1;
    }
}
```