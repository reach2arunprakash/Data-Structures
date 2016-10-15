#Sort Color II

[原題連結](http://www.lintcode.com/en/problem/sort-colors-ii/)

題意：為 [Sort Color](array/sort_color.md) 的延伸，將原本三個顏色變為K個顏色。

解題思路：

* 暴力法：用桶排序，花$$O(N)$$時間與空間
* Quick Sort：花$$O(NlogN)$$時間
* 則利用陣列的前K個空間作各顏色的計數器，此方法可達到$$O(N)$$，作法如下：
  1. 從左掃瞄到右邊，遇到一個數字，先找到對應的bucket.
  
    比如  3 2 2 1 4，第一個3對應的bucket是index = 2 (bucket從0開始計算）
  2. Bucket 如果有數字，則把這個數字移動到i的position(就是存放起來），然後把bucket記為-1(表示該位置是一個計數器，計1）。
  3. Bucket 存的是負數，表示這個bucket已經是計數器，直接減1. 並把color[i] 設置為0 （表示此處已經計算過）
  4. Bucket 存的是0，與3一樣處理，將bucket設置為-1， 並把color[i] 設置為0 （表示此處已經計算過）
  5. 回到position i，再判斷此處是否為0（只要不是為0，就一直重複2-4的步驟）。
  6. 完成1-5的步驟後，從尾部到頭部將數組置結果。（從尾至頭是為了避免開頭的計數器被覆蓋）
  
  例子(按以上步驟運算)：

    3 2 2 1 4

    2 2 -1 1 4

    2 -1 -1 1 4

    0 -2 -1 1 4

    -1 -2 -1 0 4

    -1 -2 -1 -1 0

>以上方法由 http://www.cnblogs.com/yuzhangcmu/p/4177326.html 提供


```java
public void sortColors2(int[] colors, int k) {
    
    if (colors == null || colors.length == 0 || k == 0) {
        return;
    }
    
    // 利用陣列中前k個元素作counter，負數代表該顏色有幾個
    // 如-1等於有1個，-2等於有2個，會選擇負數是因為顏色的表示皆為正
    // 若拜訪過也分到該到的bucket，則將該元素設為0
    for (int i = 0; i < colors.length; i++) {
        if (colors[i] > 0) {
            int pos = colors[i] - 1;
            if (colors[pos] <= 0) {
                colors[pos]--;
                colors[i] = 0;
            } else {
                colors[i] = colors[pos];
                colors[pos] = -1;
                i--;
            }
        }
    }
    
    //從後面作回來，因為怕蓋掉前k個counter
    int i = colors.length - 1;
    int j = k - 1; //為counter的最後一個位置
    
    while (i >= 0) {
        while (colors[j] < 0) {
            colors[j] += 1;
            colors[i] = j + 1;
            i--;
        }
        j--;
    }
}
```
