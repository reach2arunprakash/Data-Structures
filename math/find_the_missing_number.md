#Find The Missing Number

[Leetcode](https://leetcode.com/problems/missing-number/)

題意：給定一陣列有N個元素，找出在 0...N 中不在陣列裡的那個元素。

Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

For example,
Given nums = [0, 1, 3] return 2.

**Note:**
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

解題思路：

1. 先利用梯形公式把原本應該的總數算出來，接著再把元素和與總數相減即可得出消失的數，原始碼如下：

```java
public int findMissing(int[] nums) {
    
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    int len = nums.length;
    int total = len * (len + 1) / 2;
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
    }
    
    return total - sum;
}
```

>Time Complexity：$$O(N)$$

---
2.利用 ```xor``` 的特性來幫助我們解這道題，有點類似 [Single Number](array/single_number.md) 那道題，把所有元素全xor起來，接著再xor從0到N，最後把這兩個相 xor 即可得到消失的數，原始碼如下：

```java
public int findMissing(int[] nums) {
    
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    int len = nums.length;
    int total = len * (len + 1) / 2;
    int xorOne = nums[0];
    for (int i = 1; i < len; i++) {
        xorOne ^= nums[i];
    }
    
    int xorTwo = 1;
    for (int i = 2; i <= len; i++) {
        xorTwo ^= i;
    }
    
    return xorTwo ^ xorOne;
}
```
>Time Complexity：$$O(N)$$