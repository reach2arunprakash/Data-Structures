# Meeting Room

[Leetcode](https://leetcode.com/problems/meeting-rooms/)

題意：

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), determine if a person could attend all meetings.

For example,
Given [[0, 30],[5, 10],[15, 20]],
return false.

解題思路：

排序後一一比較是否有重疊即可，時間複雜度O(nlogn)，程式碼如下：

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
        public int compare (Interval iOne, Interval iTwo) {
            if (iOne.start != iTwo.start) {
                return iOne.start - iTwo.start;
            } else {
                return iOne.end - iTwo.end;
            }
        }
    }
    public boolean canAttendMeetings(Interval[] intervals) {
        if (intervals == null || intervals.length == 0) {
            return true;
        }
        Arrays.sort(intervals, new IntervalComparator());
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i - 1].end > intervals[i].start) {
                return false;
            }
        }
        return true;
    }
}
```