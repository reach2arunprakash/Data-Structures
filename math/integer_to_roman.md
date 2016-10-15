#Integer to Roman

[原題網址](http://www.lintcode.com/en/problem/integer-to-roman/)

題意：把整數轉換為羅馬數字

update on 2015.12.20

```java
public class Solution {
    public int romanToInt(String s) {
        //：Ⅰ（1）Ⅴ（5）Ⅹ（10）L（50）C（100）D（500）M（1000） 
        // rules:位於大數的後面時就作為加數；位於大數的前面就作為減數
        //eg：Ⅲ=3,Ⅳ=4,Ⅵ=6,ⅩⅨ=19,ⅩⅩ=20,ⅩLⅤ=45,MCMⅩⅩC=1980
        //"DCXXI"

        if(s == null || s.length() == 0) return 0;
        int len = s.length();
        HashMap<Character,Integer> map = new HashMap<Character,Integer>();
        map.put('I',1);
        map.put('V',5);
        map.put('X',10);
        map.put('L',50);
        map.put('C',100);
        map.put('D',500);
        map.put('M',1000);
        int result = map.get(s.charAt(len -1));
        int pivot = result;
        for(int i = len -2; i>= 0;i--){
            int curr = map.get(s.charAt(i));
            if(curr >=  pivot){
                result += curr;
            }else{
                result -= curr;
            }
            pivot = curr;
        }
        return result;
    }
}
```
```java
public String intToRoman(int n) {
        
    int[] number = new int[]{1000, 900, 500, 400, 100, 90,  50, 40, 10, 9, 5, 4, 1};
    String[] roman = new String[]{"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
    
    StringBuilder str = new StringBuilder();
    for (int i = 0; i < number.length; i++) {

        while (n >= number[i]) {
            str.append(roman[i]) ;
            n -= number[i];
        }
    }
    
    return new String(str);
    
}
```