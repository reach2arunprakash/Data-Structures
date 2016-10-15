#Triangle Count

[]()

題意：給定一個陣列，陣列中每個元素代表邊長，找出陣列中能組合成合法三角形的總數。

解題思路：合法三角形就是選三個邊，任兩邊合必大於第三邊，原本若使用暴力法需花費 $$O(N^{3})$$ 的時間複雜度。我們可以透過排序後陣列升序的特性。
原本需要檢查三次才能確保合法性，但陣列已排序過，i <= j <= k，我們只需要檢查 i + j > k即可，程式碼如下：

>需要注意的地方，我們固定第 $$i$$ 邊為三角形的最大邊，在 $$○$$ 到 $$i-1$$ 中找出所有可能性。

```java
public int triangleCount(int S[]) {
    
    if (S == null || S.length <= 2) {
        return 0;
    }
    
    Arrays.sort(S);
    int count = 0; 
    int len = S.length;
    
    for (int i = 0; i < len; i++) {
        int start = 0;
        int end = i - 1;
        while (start < end) {
            if (S[start] + S[end] > S[i]) {
                count += (end - start); // 關鍵
                end--;
            } else {
                start++;
            }
        }
    }
    
    return count;
}
```

>Time Complexity：$$O(Nlog{N} + N^{2})$$，此為花在排序的時間，與後面兩根指針運算。