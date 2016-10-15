#Merge Intervals

[]()

題意：

Given a collection of intervals, merge all overlapping intervals.

For example,

Given ```[1,3],[2,6],[8,10],[15,18]```,

return ```[1,6],[8,10],[15,18]```.

解題思路：

參考網友 [喜刷刷](http://bangbingsyb.blogspot.com/2014/11/leetcode-merge-intervals.html) 排序後並合併的解法。

首先按照start的大小來給所有interval排序，start小的在前。然後掃瞄逐個插入結果。如果發現當前interval a和結果中最後一個插入的interval b不重合，則插入a到b的後面；反之如果重合，則將a合併到b中。

>注意要給object排序需要定義一個compare structure作為sort函數的額外參數。

程式碼如下：

```java
public List<Interval> merge(List<Interval> intervals) {
        
    if (intervals == null || intervals.size() <= 1) {
        return intervals;
    }
    
    
    Collections.sort(intervals, new Comparator<Interval>() {
        public int compare(Interval intervalOne, Interval intervalTwo) {
            if (intervalOne.start != intervalTwo.start) {
                return intervalOne.start - intervalTwo.start;
            } else {
                return intervalOne.end - intervalTwo.end;
            }
        }
    });
    
    List<Interval> res = new ArrayList<Interval>();
    for (Interval interval : intervals) {
        
        // 因 res 為空或是 兩個interval不重合，則直接插入即可。
        if (res.isEmpty() || res.get(res.size() - 1).end < interval.start) {
            res.add(interval);
        } else {
            // 先把 res 尾元素拿出來與新的 interval 作比較來決定新的 interval的區間
            Interval cur = res.get(res.size() - 1);
            res.remove(res.size() - 1);
            int start = Math.min(cur.start, interval.start);
            int end = Math.max(cur.end, interval.end);
            Interval newInterval = new Interval(start, end);
            res.add(newInterval);
        }
    }
    
    return res;
}
```

updated 2015.10.9

```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
public class Solution {
    public List<Interval> merge(List<Interval> intervals) {
        List<Interval> res = new ArrayList<Interval>();
        if (intervals == null || intervals.size() == 0) {
            return res;
        }
        
        Collections.sort(intervals, new Comparator<Interval>() {
           public int compare(Interval iOne, Interval iTwo) {
               return iOne.start - iTwo.start;
           } 
        });
        
        Interval prev = intervals.get(0);
        for (int i = 1; i < intervals.size(); i++) {
            Interval cur = intervals.get(i);
            if (prev.end < cur.start) {
                res.add(prev);
                prev = cur;
            } else if (cur.end < prev.start) {
                res.add(cur);
            } else {
                int start = Math.min(prev.start, cur.start);
                int end = Math.max(prev.end, cur.end);
                prev = new Interval(start, end);
            }
        }
        res.add(prev);
        return res;
    }
}
```

---
###Reference
1. http://bangbingsyb.blogspot.com/2014/11/leetcode-merge-intervals.html