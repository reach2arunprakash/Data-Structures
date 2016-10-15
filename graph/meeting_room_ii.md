# Meeting Room II

[Leetcode](https://leetcode.com/problems/meeting-rooms-ii/)

題意：

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), find the minimum number of conference rooms required.

For example,

Given [[0, 30],[5, 10],[15, 20]],
return 2.

為[Meeting Room]() 的follow up，在此找出最少需要多少會議室。


解題思路：

首先還是先將interval排序，在此我們使用一個min heap，heap中塞的值為interval的end值，因我們只需要不斷的把新的interval去與heap中最小的end來比較，如果有衝突到，則代表與後面其他的可能都有衝到了，必再開一間房，分以下三個情況討論：

1. HEAP為空，表示前面沒會議，或是沒有衝突
2. HEAP頂端元素比當前interval的start小，表示與前面的沒有衝突到，把先結束的會議移掉，再把目前的會議放進去。
3. HEAP頂端元素比當前interval的start大，表示衝了，所以直接塞進去，前面的也拿不出來了。


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
    
    public class IntervalComparator implements Comparator<Interval> {
        @Override
        public int compare(Interval iOne, Interval iTwo) {
            if (iOne.start != iTwo.start) {
                return iOne.start - iTwo.start;
            } else {
                return iOne.end - iTwo.end;
            }
        }
    }
    
    public int minMeetingRooms(Interval[] intervals) {
        if (intervals == null || intervals.length == 0) {
            return 0;
        }
        
        Arrays.sort(intervals, new IntervalComparator());
        PriorityQueue<Integer> q = new PriorityQueue<Integer>();
        int room = 0;
        for (int i = 0; i < intervals.length; i++) {
            // 表示前面沒會議，或是沒有衝突
            if (i == 0 || q.isEmpty()) {
                room++;
                q.add(intervals[0].end);
            } else if (q.peek() <= intervals[i].start) {
                // 與前面的沒有衝突到，把最後一個移除，再把目前的放進去
                q.poll();
                q.add(intervals[i].end);
            } else {
                // 衝了，所以直接塞進去，前面的也拿不出來了。
                room++;
                q.add(intervals[i].end);
            }
        }
        
        return room;
        
    }
}
```

updated on 2016.1.12

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
    
    public class IntervalComparator implements Comparator<Interval> {
        public int compare(Interval one, Interval two) {
            if (one.start != two.start) {
                return one.start - two.start;
            } else {
                return two.end - one.end;
            }
        }
    }
    
    
    public int minMeetingRooms(Interval[] intervals) {
        if (intervals == null || intervals.length == 0) {
            return 0;
        }
        
        Arrays.sort(intervals, new IntervalComparator());
        PriorityQueue<Integer> q = new PriorityQueue<Integer>();
        
        q.offer(intervals[0].end);
        for (int i = 1; i < intervals.length; i++) {
            int val = q.peek();
            Interval in = intervals[i];
            if (val <= in.start) {
                q.poll();
            }
            q.offer(in.end);
        }
        return q.size();
    }
}
```

---
###Reference
1. http://blog.csdn.net/pointbreak1/article/details/48840671