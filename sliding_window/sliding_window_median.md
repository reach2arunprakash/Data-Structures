#Sliding Window Median

[原題連結](http://www.lintcode.com/en/problem/sliding-window-median/)

題意：Given an array of n integer, and a moving window(size k), move the window at each iteration from the start of the array, find the median of the element inside the window at each moving. (If there are even numbers in the array, return the N/2-th number after sorting the element in the window. )

解題思路：

利用一個 maxheap 來存放所有等於或小於median的集合，另利用一個 minheap 來存放所有大於 median 的集合，而 median 永遠都是 maxheap 的最大值即 peek 。這裡用了一個偷吃步就是不實作 comparator ，直接使用```java Collections.reverseOrder()```來實作 maxheap ，因原始的 heap 是最小值在最前面。接著不斷判斷當前元素應該插到 maxheap 或是 minheap 。插入後，再作調整， minheap 的元素個數不可以比 maxheap 大，因 maxheap 的 peek 是 median ，而maxheap的元素個數最多比minheap多一。

接著處理刪除的部份，在此是偷吃步直接使用內建的刪除功能，需花 $$O(N)$$，可以使用hash heap來加速刪除以達到$$O(logN)$$的時間複雜度。程式如下


```java
public ArrayList<Integer> medianSlidingWindow(int[] nums, int k) {
    
    ArrayList<Integer> res = new ArrayList<Integer>();
    
    if (nums == null || nums.length == 0) {
        return res;
    }
    
    PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(1, Collections.reverseOrder());
    PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>();
    
    for (int i = 0; i < nums.length; i++) {
        
        // 決定加入minheap或是maxheap
        // 中位數都是maxheap.peak()，即左邊的最大值
        if (maxHeap.isEmpty() || nums[i] < maxHeap.peek()) {
            maxHeap.offer(nums[i]);
        } else {
            minHeap.offer(nums[i]);
        }
        
        // 作調整，右邊永遠只能比左邊少或相等，左邊最多只能比右邊多一個元素
        if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
        if (minHeap.size() + 1 < maxHeap.size()) {
            minHeap.offer(maxHeap.poll()); 
        }
        
        if (i >= k - 1) {
            res.add(maxHeap.peek());
            int delCandidate = nums[i - k + 1];
            
            if (delCandidate <= maxHeap.peek()) {
                maxHeap.remove(delCandidate);
            } else {
                minHeap.remove(delCandidate);
            }
            
            if (minHeap.size() > maxHeap.size()) {
                maxHeap.offer(minHeap.poll());
            }
            if (minHeap.size() + 1 < maxHeap.size()) {
                minHeap.offer(maxHeap.poll());
            }
        }
    }
    
    return res;
}
```