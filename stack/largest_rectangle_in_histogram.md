#Largest Rectangle in Histogram

[原題網址](http://www.lintcode.com/en/problem/largest-rectangle-in-histogram/#)

題意：給定一個陣列，陣列中的每個值代表柱子的高度，找出最大的四邊形area。

解題思路：

1. 暴力法：以每個柱子往右邊掃，掃到第一個比他小的柱子則停下來，寬度則為柱子到第一個比他小的柱子前一個，高度則為當前柱子的高度，因它為最低，需花$$O(N^{2})$$，程式碼如下：

```java
public int largestRectangleArea(int[] height) {
    int len = height.length;
    int result = 0;
    if (len == 0) {
        return result;
    }
    for (int start = 0; start < len; start++) {
        int length = height[start];
        for (int end = start; end < len; end++) {
            int width = end - start + 1;
            length = Math.min(length, height[end]);
            if (result <= width * length) {
                result = width * length;
            }
        }
    }
    
    return result;
}
```
2. Stack法：此方法的思路就是，以每根柱子為中心，向左找第一根比他矮的柱子，向右找第一根比他矮的柱子。我們可以利用 **增序stack** 的特性來幫助我們以線性時間完成這個任務。


掌握以下三個條件：
1. stack只以升序方式來存放，且是存放陣列的index。
2. 每次pop時便計算一次area，push時則不需作任何事。
2. 當stack 為空或是stack.peek()的值比當前值還小的時候，表示目前還找不到右邊第一個比當前數還小的元素，因此把當前數放進 stack中。
2. 當stack不為空時，且當前數比 ```height[stack.peek()]``` 還大時，則我們已經找到了 ```stack.peek()``` ，所指的那根柱子的左右第一個比他矮的柱子：
    1. 右邊第一個比他小的數：即為當前的 ```height[i]```
    2. 左邊第一個比他小的數，有兩個情況
        1. stack為空了，表示左邊沒有比他還小的數，則寬度就是i。
        2. stack不為空，則左邊第一個比他矮的就是目前stack頂端的元素。寬度為 ```i - stack.peek() - 1```

程式碼如下：

```java
public int largestRectangleArea(int[] height) {
    if (height == null || height.length == 0) {
        return 0;
    }
    
    // 以每一個點為中心，找出左右第一位比該點小的值
    // 使用 stack來輔助，若stack不為升序的話，則不斷pop，每一次pop即可得到兩個紀錄
    // 使得最上面那個點被彈出的值為被彈出的點之右邊第一個小的值。
    // 被pop那個點的下一個，則否左邊第一個比該點小的值
    // stack塞的數為index
    int max = 0;
    Stack<Integer> stack = new Stack<Integer>();
    
    for (int i = 0; i <= height.length; i++) {
        int cur = (i == height.length) ? -1 : height[i];
        // 記得while是把stack中第一個值當index來抓出height來比
        while (!stack.isEmpty() && cur < height[stack.peek()]) {
            // 直接用pop出來的index來取出高
            // 如果stack為empty，則代表該點右邊沒有最小的，即走到頭，所以設為i
            // 若不為empty，則把彈出後的stack取最上面的值去減i來得到寬
            int h = height[stack.pop()];
            int w = stack.isEmpty() ? i : i - stack.peek() - 1;
            max = Math.max(max, h * w);
        }
        stack.push(i);
    }
    
    return max;
}
```

---
###Reference
1. http://www.cnblogs.com/lichen782/p/leetcode_Largest_Rectangle_in_Histogram.html