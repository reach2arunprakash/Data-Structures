#Trailing Zeros

題意：給予一個數 $$n$$ ，求 $$n!$$ 有多少個零。

解題思路：會構成 $$0$$ 只有兩個 factor，5 與 2，由於 2 的個數一定比 5 多，因此只需要求有幾個5就好了，但是不能直接只除以5，5 的倍數 25，125，625…皆要考慮進去。

原本使用以下解法會出現超時的錯誤

```java
public long trailingZeros(long n) {
    long count = 0;
    for (int i = 5; i <= n; i = i * 5) {
        count += n / i;
    }
    return count;
}
```

因此改用遞迴的方式實作，程式碼如下：

```java
public long trailingZeros(long n) {
    
    if (n < 5) {
        return 0;
    }
    
    return n / 5 + trailingZeros(n / 5);
}
```