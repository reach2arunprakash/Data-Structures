#Compare Strings

解題思路：利用一個陣列代表字串中出現的字元，若出現在 s 字串，則陣列該位置加一，若出現在 t 字串，則陣列該位置減一，最後利用一個循環確保陣列中每個值皆為零即可。

```java
private boolean isAnagram(String s, String t) {

    if (s.length() != t.length()) {
        return false;
    }

    int[] map = new int[256];
    for (int i = 0; i < s.length(); i++) {
        map[s.charAt(i) - 'a']++;
        map[t.charAt(i) - 'a']--;
    }

    for (int i = 0; i < 256; i++) {
        if (map[i] != 0) {
            return false;
        }
    }

    return true;
}
```

>Time Complexity：O(n)