#Insert Interval

[原題網址](http://www.lintcode.com/en/problem/insert-interval/)

題意：

Given a non-overlapping interval list which is sorted by start point.

Insert a new interval into it, make sure the list is still in order and non-overlapping (merge intervals if necessary).

Example
Insert ```[2, 5]``` into ```[[1,2], [5,9]]```, we get ```[[1,9]]```.

Insert ```[3, 4]``` into ```[[1,2], [5,9]]```, we get ```[[1,2], [3,4], [5,9]]```.

解題思路：

遍歷每個原本陣列中的所有區間，不斷與新的區間作比較，分成以下三種情況作處理

1. 當前區間在新區間前面，且兩者不重疊，則直接把當前區間放入結果。
2. 當前區間在新區間後面，且兩者不重疊，則把新區間放入結果並更新新區間為舊區間。
3. 當兩者重疊，則分別找出兩個區間的最小值與最大值，並建立新的區間，以合併兩個區間。
    - 在此只是先把 newInterval 更新並末加入 list 中，待下一次再檢查


> 因很有可能newInterval 比 intervals 的最後一個還大，或是第三個狀況更新了 newInterval 而還沒加入，記得在最後把 newInterval 加入到 list中。

程式碼如下：

```java
public ArrayList<Interval> insert(ArrayList<Interval> intervals, Interval newInterval) {
    ArrayList<Interval> result = new ArrayList<Interval>();
    
    if (intervals == null || intervals.size() == 0) {
        result.add(newInterval);
        return result;
    }
    
    for (Interval interval : intervals) {
        // 當目前的區間在新區間的前面時，且沒有重疊，則直接把當前的區間放入結果中。
        if (interval.end < newInterval.start) {
            result.add(interval);
        } else if (interval.start > newInterval.end) {
            result.add(newInterval);
            newInterval = interval;
        } else if (newInterval.start <= interval.end || newInterval.end >= interval.start) {
            // 在此只是先把 newInterval更新並末加入 list中
            int start = Math.min(newInterval.start, interval.start);
            int end = Math.max(newInterval.end, interval.end);
            newInterval = new Interval(start, end);
        }
    }
    
    // 因很有可能newInterval 比 intervals 的最後一個還大，
    // 或是第三個狀況更新了 newInterval 而還沒加入
    // 記得在最後把 newInterval 加入到 list中。
    result.add(newInterval);
    
    return result;
}
```
>Time Complexity：$$O(N)$$
---
###Reference
1. http://www.cnblogs.com/springfor/p/3872333.html

