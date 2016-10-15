# Data Stream Median
[原題連結](http://www.lintcode.com/en/problem/data-stream-median/)

題意：求陣列中每個階段的中位數，Result[i]即求nums[0]到nums[i]的中位數，因題目只要求取出$$A[(n - 1) / 2]$$，因此無需多作處理，有的題目會要求偶數個數時需求出 $$(A[(n - 1) / 2] + A[(n - 1) / 2 + 1]) / 2$$。

解題思路：詳述於註解中，程式碼如下。

```java
public int[] medianII(int[] nums) {

    int len = nums.length;
    int[] result = new int[len];
    if (nums == null || nums.length == 0) {
        return result;
    }
    
    
    // 解法：使用兩個heap(min-heap, max-heap)，min-heap存放左半邊包含中位數，
    // max-heap存放右半邊，所以max-heap的大小最多只能比min-heap多一(如果array size是奇數).
    // 不斷的維護此size差，即把某heap中的某數移到另一個heap。
    // max-heap存放比median小的數
    // min-heap存放比median大的數
    // 因priority queue預先是越小的越前面，所以實作max heap時，存負數，要拿出來再轉正。
    
    
    PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>();
    PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>();
    int median = nums[0];
    result[0] = median;
    
    for (int i = 1; i < len; i++) {
        if (nums[i] < median) {
            maxHeap.add(-nums[i]);
        } else {
            minHeap.add(nums[i]);
        }
        
        if (maxHeap.size() > minHeap.size()) {
            minHeap.add(median);
            median = -maxHeap.peek();
            maxHeap.poll();
        } else if (maxHeap.size() + 1 < minHeap.size()) {
            maxHeap.add(-median);
            median = minHeap.peek();
            minHeap.poll();
        }
        result[i] = median;
    }
    return result;
}
```

Leetcode 版，要求我們實作add與find這兩個function，整體思路就是先加再來作調整。程式碼如下：

```java
class MedianFinder {
    PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>();
    PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(10, Collections.reverseOrder());
    // Adds a number into the data structure.
    public void addNum(int num) {
       if (maxHeap.isEmpty() || maxHeap.peek() > num) {
           maxHeap.offer(num);
       } else {
           minHeap.offer(num);
       }
       
       if (maxHeap.size() < minHeap.size()) {
           maxHeap.offer(minHeap.poll());
       }
       if (minHeap.size() + 1 < maxHeap.size()) {
           minHeap.offer(maxHeap.poll());
       }
    }

    // Returns the median of current data stream
    public double findMedian() {
        if (maxHeap.size() - minHeap.size() == 1) {
            return maxHeap.peek();
        } else {
            int medianOne = maxHeap.peek();
            int medianTwo = minHeap.peek();
            return (medianOne + medianTwo) / 2.0;
        }
    }
};

// Your MedianFinder object will be instantiated and called as such:
// MedianFinder mf = new MedianFinder();
// mf.addNum(1);
// mf.findMedian();
```