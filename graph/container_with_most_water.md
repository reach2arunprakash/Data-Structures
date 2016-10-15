#Container With Most Water

[原題連結](http://www.lintcode.com/en/problem/container-with-most-water/)

題意：給定一個陣列，陣列中的每個元素代表在位置 i 的牆高，找出兩道牆使得該容器可以裝最多水

解題思路：

* **暴力法**：可使用兩條 for 循環來枚舉所有牆的組合來找到高的組合，但需花費 $$O(N^{2})$$。


```java
public int maxArea(int[] heights) {
    if (heights == null || heights.length < 0) {
        return 0;
    }
    
    int len = heights.length;
    int maxCapacity = 0;
    
    for (int i = 0; i < len - 1; i++) {
        for (int j = i + 1; j < len; j++) {
            int height = Math.min(heights[i], heights[j]);
            int capacity = height * (j - i);
            maxCapacity = (capacity > maxCapacity) ? capacity : maxCapacity;
        }
    }
    
    return maxCapacity;
}
```
* **排序**：對高度作排序，因 index 經過排序後順序會亂掉，因此需要自創一個屬性來紀錄原本的 index ，接著由後往前解，因為最高牆在最後面。
* **兩個指針**：使用前後兩根指針來計算，每次移動較小的指針，因如果小的指針不移動的話，不管較大指針怎麼移動，都不會改變水位的高度。程式碼如下：

```java
public int maxArea(int[] heights) {
    
    if (heights == null || heights.length == 0) {
        return 0;
    }
    
    int left = 0;
    int right = heights.length - 1;
    while (heights[left] == 0) {
        left++;
    }
    
    while(heights[right] == 0) {
        right--;
    }
    
    int capacity = Math.min(heights[left], heights[right]) * (right - left);
    while (left < right) {
        int curCapacity = 0;
        if (heights[right] > heights[left]) {
            left++;
        } else {
            right--;
        }
        curCapacity = Math.min(heights[left], heights[right]) * (right - left);
        capacity = curCapacity > capacity ? curCapacity : capacity;
    }
    
    return capacity;
}
```
>Time Complexity： $$O(N)$$，因只需遍歷陣列一次。

---
###Reference
1. http://www.cnblogs.com/TenosDoIt/p/3812880.html
