# Single Number II

[原題連結](http://www.lintcode.com/en/problem/single-number-ii/)

題意：此為 [Single Number](array/single_number.md) 的變形，此時每個元素皆出現三次，只有一個元素出現一次，找出那個元素。

解題思路：這一題就無法用上一題的 XOR 解法，因每個元素皆出現三次，但我們可以自己實作三進制，搭配上不進位加法來模擬三進制的 XOR 。

作法：
+ 把每個數轉成三進制
+ 接著用不進位加法
+ 再轉回十進制

程式碼如下：

```java
public int singleNumberII(int[] A) {
    if (A == null || A.length == 0) {
        return -1;
    }
    
    // get the power of 3
    int[] pow3 = new int[20];
    pow3[0] = 1;
    for (int i = 1; i < 20; i++) {
        pow3[i] = pow3[i - 1] * 3;
    }
    
    // at most 20 bits for base 3 in maxint
    int[] bits = new int[20]; 
    
    for (int i = 0; i < 20; i++) {
        for (int j = 0; j < A.length; j++) {
            bits[i] += A[j] / pow3[i] % 3;
        }
    }
    
    // convert to decimal
    int result = 0;
    for (int i = 0; i < 20; i++) {
        result += pow3[i] * (bits[i] % 3);
    }
    return result;
}
```

---
另有一解法是根據陣列中每個元素的每個位元作處理，將個數的每個元位作加總後再取模3，使用推來推去法把要處理的位元取出來並與結果合併。

假如陣列為[5,5,5,3,3,3,4]， single number 為 4

| Decimal | Binary | 
| ------- |:----:| 
| 5       | 1001 |
| 5       | 1001 |
| 5       | 1001 |
| 3       | 0011 |
| 3       | 0011 |
| 3       | 0011 |
| 4       | 0100 |

加總後等於：3146，接著每個位元取mod 3，會變成0110，轉為十進制後，答案為4。



```java
public int singleNumberII(int[] A) {
    int result = 0;
    int[] bit = new int[32];
    for (int i = 0; i <32; i++) {
        for (int j = 0; j < A.length; j++) {
            // 把A array中的每個數取出第i個binary值作相加再除以3
            bit[i] += (A[j] >> i) & 1; 
            bit[i] = bit[i] % 3;
        }
        // 接著把bit合成result即可
        // 模3後的結果可以直接傳回二進位，因為每個數皆出現三次，且只有一個數出現一次
        // 不會有出現2次的，所以模三的結果不是1就是0; 
        result = result | (bit[i] << i);
    }
    return result;
}
```
