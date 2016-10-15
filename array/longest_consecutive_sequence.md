# Longest Consecutive Sequence

[Leetcode](https://leetcode.com/problems/longest-consecutive-sequence/)

題意：

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

For example,

Given [100, 4, 200, 1, 3, 2],

The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.

Your algorithm should run in O(n) complexity.

解題思路：

通常不排序又需要O(n)的解法會需要用到hashmap或是hashset，此題就是個例子，我們先將所有數加入set中，接著不斷往前與往後探索最長的長度，其程式碼如下：


```java
public class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        HashSet<Integer> set = new HashSet<Integer>();
        
        for (int i = 0; i < nums.length; i++) {
            set.add(nums[i]);
        }
        
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            if (set.contains(nums[i])) {
                int count = 1;
                int next = nums[i] - 1;
                while (set.contains(next)) {
                    set.remove(next);
                    count++;
                    next--;
                    
                }
                
                next = nums[i] + 1;
                while (set.contains(next)) {
                    set.remove(next);
                    count++;
                    next++;
                    
                }
                max = (count > max) ? count : max;
            }
        }
        return max;
    }
}
```

updated 2015.11.24

上面的方法會超時，因為太多重複計算的部份，我們利用只計算沒被造訪過的點。

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return an integer
     */
    public int longestConsecutive(int[] num) {
        // write you code here
        int len = num.length;
        if (num == null || num.length == 0) {
            return 0;
        }
        //先把所有array的值全塞進hashmap裡，0代表尚末拜訪過。
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < len; i++) {
            map.put(num[i], 0);
        }
        
        int max = 1;
        for (int number : num) {
            //為了不重複計算，只要拜訪過(值為1)即跳下一個number
            if (map.get(number) == 1) {
                continue;
            }
            //把陣列中每一個數拿出來不斷去往前往後比
            //有找到連續的值就不斷加一
            int curMax = 1;
            int i = number;
            while (map.containsKey(i+1)) {
                map.put(i+1, 1);
                i++;
                curMax++;
            }
            //重設i，接著繼續往前找
            i = number;
            while (map.containsKey(i-1)) {
                map.put(i-1, 1);
                i--;
                curMax++;
            }
            
            max = (curMax > max) ? curMax : max;
        }
        return max;
    }
}

```
---
###Reference
1. http://blog.csdn.net/fightforyourdream/article/details/15024861