#Gray Code

[]()

題意：

格雷編碼是一個二進制數字系統，在該系統中，兩個連續的數值僅有一個二進制的差異。

給定一個非負整數 n ，表示該代碼中所有二進制的總數，請找出其格雷編碼順序。一個格雷編碼順序必須以 0 開始，並覆蓋所有的 $$2^{n}$$ 個整數。

樣例
給定 n = 2， 返回 ```[0,1,3,2]```。其格雷編碼順序為：

```
00 - 0
01 - 1
11 - 3
10 - 2
```

解題思路：

以下為網友 [愛作飯的小瑩子](http://www.cnblogs.com/springfor/p/3889222.html) 提供的思路

可以看到n位的格雷碼由兩部分構成，一部分是n-1位格雷碼，再加上 1<<(n-1)和n-1位格雷碼的逆序（整個格雷碼逆序)

1位格雷碼有兩個碼字 
(n+1)位格雷碼中的前2^n個碼字等於n位格雷碼的碼字，按順序書寫，加前綴0 
(n+1)位格雷碼中的後2^n個碼字等於n位格雷碼的碼字，按逆序書寫，加前綴1。

由於是二進制，在最高位加0跟原來的數本質沒有改變，所以取得上一位算出的格雷碼結果，再加上逆序添1的方法就是當前這位格雷碼的結果了。

n = 0時，[0]

n = 1時，[0,1]

n = 2時，[00,01,11,10]

n = 3時，[000,001,011,010,110,111,101,100]

其程式碼如下：

```java
public ArrayList<Integer> grayCode(int n) {
        
    ArrayList<Integer> res = new ArrayList<Integer>();
    
    if (n < 0) {
        return res;
    }
    
    res.add(0);
    for (int i = 0; i < n; i++) {
        int length = res.size();
        int hiBit = 1 << i;
        
        for (int j = length - 1; j >= 0; j--) {
            res.add(res.get(j) + hiBit);
        }
    }
    
    return res;
}
```

---
###Reference
1. http://www.cnblogs.com/springfor/p/3889222.html


