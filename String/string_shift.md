# String Shift

input: String, key(int value),
e.g. input: "aAa！"， key = 3, return "dDd!"



```java
public String shift(String s, int key) {
    String res = "";
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);.
        if (c >= 'a' && c <= 'z') {
            res = res + (char) ((c - 'a' + key) % 26 + 'a');
        } else if (c >= 'A' && c <= 'Z') {
            res = res + (char) ((c - 'A' + key) % 26 + 'A');
        } else {
            res = res + c;
        }
    }
    return res;
}

```