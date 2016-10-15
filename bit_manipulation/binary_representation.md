# Binary Representation

[]()

題意：



解題思路：

把字串透過"."分成整數與小數，再分別處理，整數的部份不斷的用除，小數的部份不斷的用乘，只要乘後大於等於1.0則append 1，否則append 0

>記得整數的部份要insert在string的頭

自己寫了一個非常長的code，主要是parse整數與小數的部份，以茲紀念，其程式碼如下：

```java
public class Solution {
    /**
     *@param n: Given a decimal number that is passed in as a string
     *@return: A string
     */
    public String binaryRepresentation(String n) {
        
        if (n == null || n.length() == 0) {
            return "";
        }
        
        String[] strs = n.split("\\.");
        int integer;
        double fraction;
        if (strs.length == 2) {
            integer = Integer.parseInt(strs[0]);
            StringBuilder fractionSB = new StringBuilder(strs[1]);
            fractionSB.insert(0, "0.");
            fraction = Double.parseDouble(fractionSB.toString());
        } else if (strs.length == 1){
            integer = Integer.parseInt(strs[0]);
            fraction = 0.0;
        } else {
            return "";
        }
        
        StringBuilder intStr = new StringBuilder();
        while(integer > 0) {
            intStr.insert(0, integer % 2);
            integer /= 2;
        }
        
        StringBuilder fraStr = new StringBuilder();
        while(fraction > 0.0) {
            fraction = fraction * 2;
            int intPart = (int) fraction;
            fraStr.append(intPart);
            // 注意，這裡要大於等於1.0 否則會無限循環
            if (fraction >= 1.0) {
                fraction -= 1.0;
            }
            if (fraStr.length() > 32) {
                return "ERROR";
            }
        }
        
        StringBuilder res = new StringBuilder();
        if (intStr.length() > 0) {
            res.append(intStr);
        } else {
            res.append("0");
        }
        if (fraStr.length() > 0) {
            res.append(".");
            res.append(fraStr);
        }

        return res.toString();
    }
}
```

網友 [Edward Liu](http://www.cnblogs.com/EdwardLiu/p/4273813.html) 提供了非常簡潔的作法，兩行就搞定了我parse 整數與小數的部份。

>int intPart = Integer.parseInt(n.substring(0, n.indexOf('.')));

>double decPart = Double.parseDouble(n.substring(n.indexOf('.')));

```java
public class Solution {
    /**
     *@param n: Given a decimal number that is passed in as a string
     *@return: A string
     */
    public String binaryRepresentation(String n) {
        int intPart = Integer.parseInt(n.substring(0, n.indexOf('.')));
        double decPart = Double.parseDouble(n.substring(n.indexOf('.')));
        String intstr = "";
        String decstr = "";
        
        if (intPart == 0) intstr += '0';
        while (intPart > 0) {
            int c = intPart % 2;
            intstr = c + intstr;
            intPart = intPart / 2;
        }
       
        while (decPart > 0.0) {
            if (decstr.length() > 32) return "ERROR";
            double r = decPart * 2;
            if (r >= 1.0) {
                decstr += '1';
                decPart = r - 1.0;
            }
            else {
                decstr += '0';
                decPart = r;
            }
        }
        return decstr.length() > 0? intstr + "." + decstr : intstr;
    }
}
```

---
###Reference
1. http://www.cnblogs.com/EdwardLiu/p/4273813.html