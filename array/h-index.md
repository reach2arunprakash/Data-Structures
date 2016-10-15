#H-Index

[原題網址](https://leetcode.com/problems/h-index/)

題意：
A scientist has index h if h of his/her N papers have at least h citations each, and the other N − h papers have no more than h citations each.

解題思路：

1. 將其發表的所有SCI論文按被引次數從高到低排序；
2. 從前往後查找排序後的列表，直到某篇論文的序號大於該論文被引次數
3. 所得序號減一即為H指數。

```java
public int hIndex(int[] citations) {
    
    if (citations == null || citations.length == 0) {
        return 0;
    }
    
    int len = citations.length;
    Arrays.sort(citations);
    for (int i = 0; i < len; i++) {
        if (citations[i] >= len - i) {
            return len - i;
        }
    }
    return 0;
}
```