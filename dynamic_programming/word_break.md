#Word Break

```java
public class Solution {
    /**
     * @param s: A string s
     * @param dict: A dictionary of words dict
     */
    public boolean wordBreak(String s, Set<String> dict) {

        if (s == null || s.length() == 0) {
            return true;
        }
        
        boolean[] res = new boolean[s.length() + 1];
        Arrays.fill(res, false);
        res[0] = true;
        int max = getMaxLength(dict);
        
        for (int i = 1; i <= s.length(); i++) {
            //取j=1與j<=i是因為java substring吃不到第i個char
            for (int j = 1; j <= i && j <= max; j++) {
                if (!res[i - j]) {
                    continue;
                }
                String curString = s.substring(i - j, i);
                if (dict.contains(curString)) {
                     //有則break省時間
                     res[i] = true;
                     break;
                }
            }
        }
        
        return res[s.length()];
    }
    
    //會超時，因此先算好dict裡面最長的字串是哪個
    private int getMaxLength(Set<String> dict) {
    
        int max = 0;
        for (String s : dict) {
            if (max < s.length()) {
                max = s.length();
            }
        }
        
        return max;
    }
}
```