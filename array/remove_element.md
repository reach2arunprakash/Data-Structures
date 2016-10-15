# Remove Element

[Lintcode](http://www.lintcode.com/en/problem/remove-element/)
[Leetcode](https://leetcode.com/problems/remove-element/)

題意：

Given an array and a value, remove all instances of that value in place and return the new length.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.


解題思路：

Updated on 2015.12.20

```java
public class Solution {
    public int removeElement(int[] nums, int val) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int pos = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != val) {
                nums[pos++] = nums[i];
            }
        }
        
        return pos;
    }
}
```

使用兩根指針 i 與 j ，j負責走訪整個陣列，i負責指向最後一個不重複的元素，程式碼如下：

```java
public class Solution {
    /** 
     *@param A: A list of integers
     *@param elem: An integer
     *@return: The new length after remove
     */
    public int removeElement(int[] A, int elem) {
        int i = 0;
        int j = 0;
        while (j < A.length) {
            if (A[j] == elem) {
                j++;
            } else {
                A[i] = A[j];
                j++;
                i++;
            }
        }
        return i;
    }
}


```