#Divide Two Integer

[Lintcode](http://www.lintcode.com/en/problem/divide-two-integers/)

題意：兩數相除，不能使用乘法，只能使用加減法。

解題思路：

暴力法，不斷的拿除數去減被除數，直到被除數比除數小，複雜度是$$(2^{31})$$，我們可以利用位運算加上加減法來幫助我們處理這道題，因為任何數皆可以轉換為二進位的表示法。

我們不斷的拿最接近被除數的 power of 2去減掉被除數，接著再把該power數加到result中，一直不斷這麼作，直到被除數比除數小，此法的複雜度為 $$O(logN)$$

雖然解法不難，但是很多邊際條件與溢位的問題需要處理

> 在這裡必須先取long再abs，否則int的最小值abs後也是原值
> result 要用 long 來避免 overflow

網友 [Yu Zhang](http://www.cnblogs.com/yuzhangcmu/p/4049170.html) 提供了以下的巧妙的檢查回傳正負結果的方式：

```result = (((dividend ^ divisor) >> 31) & 1) == 1 ? -result: result;```

其程式碼如下，搞了老半天，仍是會TLE：

```java
public int divide(int dividend, int divisor) {
    
    // 因不能除以0
    if (divisor == 0) {
        return 0;
    }
    
    // 為了應付lintcode極端會TLE的case
    if (dividend == Integer.MAX_VALUE) {
        if (divisor == -1) {
            return Integer.MIN_VALUE;
        } else if (divisor == 1) {
            return Integer.MAX_VALUE;
        }
        
    }
    
    if (dividend == Integer.MIN_VALUE) {
        if (divisor == 1) {
            return Integer.MIN_VALUE;
        } else if (divisor == -1) {
            return Integer.MAX_VALUE;
        }
    }
    
    // 先轉正整數來處理，之後再轉回來
    long p = Math.abs((long)dividend);
    long q = Math.abs((long)divisor);
    
    // 要用 long 來避免 overflow
    long result = 0;
    
    // 這裡必須是 = 因為相等時也可以減
    while ( p >= q) {
        int count = 0;
        while (p >= (p << count)) {
            count++;
        }
        
        result += 1 << (count - 1);
        p -= q << (count - 1);
    }
    
    // 除了dividend與divisor同為正數或同為負數則返回正結果，否則返回負結果
    result = (((dividend ^ divisor) >> 31) & 1) == 1 ? -result: result;
    
    // 若結果溢位，則返回最大值
    if (result > Integer.MAX_VALUE || result < Integer.MIN_VALUE) {
        return Integer.MAX_VALUE;
    }
    
    return (int) result;
    
}
```

下面為 [九章算法](http://www.jiuzhang.com/solutions/divide-two-integers/) 提供的解法：

```java
public int divide(int dividend, int divisor) {
        
    if (divisor == 0) {
        return dividend >= 0 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
    }
    
    if (dividend == 0) {
        return 0;
    }
    
    if (dividend == Integer.MIN_VALUE && divisor == -1) {
        return Integer.MAX_VALUE;
    }
    
    boolean isNegative = (dividend < 0 && divisor > 0) ||
                         (dividend > 0 && divisor < 0);
                         
    long a = Math.abs((long)dividend);
    long b = Math.abs((long)divisor);
    int result = 0;
    
    while (a >= b) {
        int shift = 0;
        while (a >= (b << shift)) {
            shift++;
        }
        
        a -= b << (shift - 1);
        result += 1 << (shift - 1);
    }
    
    return isNegative ? -result : result;
}
```


---
###Reference
1. http://www.cnblogs.com/yuzhangcmu/p/4049170.html
2. http://www.jiuzhang.com/solutions/divide-two-integers/