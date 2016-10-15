# Search in a Big Sorted Array

[Lintcode](http://www.lintcode.com/en/problem/search-in-a-big-sorted-array/)

題意：

陣列太大無法知道end，要如何找到要的 target。

解題思路：

使用不斷延伸去探測 end在哪裡，再用binary search 即可。

程式碼如下：

```java
/**
 * Definition of ArrayReader:
 * 
 * class ArrayReader {
 *      // get the number at index, return -1 if not exists.
 *      public int get(int index);
 * }
 */
public class Solution {
    /**
     * @param reader: An instance of ArrayReader.
     * @param target: An integer
     * @return : An integer which is the index of the target number
     */
    public int searchBigSortedArray(ArrayReader reader, int target) {
        
        int start = 0;
        int end = 0;
        while ((reader.get(end) != -1) && (reader.get(end) < target)) {
            end = end * 2 + 1;
        } 
        
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (reader.get(mid) < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        if (reader.get(start) == target) {
            return start;
        } else if (reader.get(end) == target) {
            return end;
        } else {
            return -1;
        }
        
    }
}

```

---
### Reference
1. http://www.jiuzhang.com/solutions/search-in-a-big-sorted-array/