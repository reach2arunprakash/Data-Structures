# Single Number

[原題連結](http://www.lintcode.com/en/problem/single-number/)

題意：給定一個陣列，其中每個元素皆出現兩次，只有一個元素出現一次，找出那個元素。

解題思路：此題可用暴力法加一個hashmap完成，但是需要花到$$O(N)$$的空間複雜度，因此我們使用了XOR的特性來幫助我們解決這道題，因 ``` a xor a = 0```，所以我們只需要把所有數全XOR起來就是答案了。程式碼如下：

```java
public int singleNumber(int[] A) {
    if (A.length == 0) {
        return 0;
    }

    int n = A[0];
    for(int i = 1; i < A.length; i++) {
        n = n ^ A[i];
    }

    return n;
}
```

>Time Complexity：$$O(N)$$，因只需遍歷一次陣列即可。

updated on 2016.1.9

O(NlogN)的解法，先排序後再兩個兩個比較

```java
public class Solution {
    public int singleNumber(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        Arrays.sort(nums);
        for (int i = 1; i < nums.length; i = i + 2) {
            if (nums[i - 1] != nums[i]) {
                return nums[i - 1];
            }
        }
        
        return nums[nums.length - 1];
    }
}
```