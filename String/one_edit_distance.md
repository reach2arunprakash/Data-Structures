#One Edit Distance

[原題網址](https://leetcode.com/problems/one-edit-distance/)

題意：給兩個字串，找出兩者是否只差一個編輯距離

解題思路：這題也可使用Edit Distance以動態規畫來解決，但是需花費$$O(N^{2})$$的時間與空間複雜度，下面為$$O(N)$$的解法，程式碼如下：

```java
public boolean isOneEditDistance(String s, String t) {
    
    // 把較短的字串排在前面
    if (s.length() > t.length()) {
        String temp = s;
        s = t;
        t = temp;
    }
    
    // 如果兩者長度差到2以上的話，代表兩者一定不是 One Edit Distance
    if (t.length() - s.length() > 1) {
        return false;
    }
    
    //用此變數來紀錄之前是否有不同過
    boolean diffBefore = false;
    
    for (int i = 0, j = 0; i < s.length(); i++, j++) {
        if (s.charAt(i) != t.charAt(j)) {
            if (diffBefore) {
                return false;
            }
            
            diffBefore = true;
            
            // 當不同時，且s較短，代表t字串可能多了一個字，但其他地方皆相同，
            // 因此不移動i指針繼續下面的比較。
            // 如ab, abc
            if (s.length() < t.length()) {
                i--;
            }
        }
        
    }
    // 若走到這一步了且diffbefore為真，則有可能是 One Edit Distance
    // 若diffBefore為假，則需再作進一步的比較長度，以避免aa與aab的問題。
    return diffBefore || s.length() < t.length();
}
```
>Time Complexity：$$O(N)$$

>Space Complexity：$$O(N)$$


updated on 2016.1.13

```java
public class Solution {
    public boolean isOneEditDistance(String s, String t) {
        if(Math.abs(s.length()-t.length()) > 1) {
            return false;
        }
        
        if(s.length() == t.length()) {
            return isOneModify(s,t);
        }
        if(s.length() > t.length()) {
            return isOneDel(s,t);
        }
        
        return isOneDel(t,s);
    }
    
    public boolean isOneDel(String s,String t){
        for(int i = 0,j = 0; i < s.length() && j < t.length(); i++, j++){
            if(s.charAt(i) != t.charAt(j)){
                return s.substring(i + 1).equals(t.substring(j));
            }
        }
        return true;
    }
    
    public boolean isOneModify(String s, String t){
        int diff = 0;
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) != t.charAt(i)) 
                diff++;
        }
        return diff == 1;
    }
}
```

---
###Reference
1. http://www.cnblogs.com/higerzhang/p/4185887.html