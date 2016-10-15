# Zigzag Iterator

[Leetcode](https://leetcode.com/problems/zigzag-iterator/)

題意：

Given two 1d vectors, implement an iterator to return their elements alternately.

For example, given two 1d vectors:
```
v1 = [1, 2]
v2 = [3, 4, 5, 6]
```
By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: ```[1, 3, 2, 4, 5, 6]```.

Follow up: What if you are given k 1d vectors? How well can your code be extended to such cases?

Clarification for the follow up question - Update (2015-09-18):
The "Zigzag" order is not clearly defined and is ambiguous for k > 2 cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic". For example, given the following input:
```
[1,2,3]
[4,5,6,7]
[8,9]
```
It should return ```[1,4,8,2,5,9,3,6,7]```.


解題思路：

使用一個q來維護所有的iterator，一但一個iterator被存取過了，他會被poll出來並檢查是否還有下一個元素，若有的話，則往q的後面塞回去。


```java
public class ZigzagIterator {
    
    private Queue<Iterator<Integer>> q;
    
    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        q = new LinkedList<Iterator<Integer>>();
        if (!v1.isEmpty()) {
            q.offer(v1.iterator());
        }
        if (!v2.isEmpty()) {
            q.offer(v2.iterator());
        }
    }

    public int next() {
        Iterator cur = q.poll();
        int res = (Integer) cur.next();
        if (cur.hasNext()) {
            q.offer(cur);
        }
        return res;
    }

    public boolean hasNext() {
        return !q.isEmpty();
    }
}

/**
 * Your ZigzagIterator object will be instantiated and called as such:
 * ZigzagIterator i = new ZigzagIterator(v1, v2);
 * while (i.hasNext()) v[f()] = i.next();
 */
```
###Reference
1. https://leetcode.com/discuss/63037/simple-java-solution-for-k-vector