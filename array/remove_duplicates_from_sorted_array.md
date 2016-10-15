# Remove Duplicates from Sorted Array I
[原題網址](http://www.lintcode.com/en/problem/remove-duplicates-from-sorted-array/)

最暴力的解法就是新建一個array，每個值只加一次，但是這需要花 O(n) 的空間複雜度，因此有一個 in space 的解法是利用兩個指針 (pos 與 i)， pos 負責遍歷整個 array ， i 負責維護 unique element 的最後一個位置，如果遇到與前一個相同的 element 則直接移動 pos ，否則將該 pos 的值 assign 給 i 的位置。


updated on 2016.1.7

```java
public class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int pos = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i - 1] != nums[i]) {
                nums[pos++] = nums[i];
            }
        }
        return pos;
    }
}
```


updated on 2015.12.20
```java
public class Solution {
    public int removeDuplicates(int[] nums) {
        // write your code here
        if (nums == null || nums.length < 2) {
            if (nums != null) {
                return nums.length;
            } else {
                return 0;
            }
        }
        //use two pointer approach
        //first pointer point to last non duplicate element
        //second pointer travse whole list.
        int pos = 1;
        for (int i = 0; i < nums.length; i++) {
            if (i != 0 && nums[i - 1] != nums[i]) {
                nums[pos++] = nums[i];
            }
        }
        // since index is start from 0
        return pos;
    }
}
```


# Remove Duplicates from Sorted Array II

[原題網址](http://www.lintcode.com/en/problem/remove-duplicates-from-sorted-array-ii/)
>Follow up for "Remove Duplicates":

>What if duplicates are allowed at most twice?

解題思路：

此題解法與上個類似，但需要增加一個變數來紀錄重複的次數。

```java
public int removeDuplicates(int[] nums) {
        
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    // pos 用來紀錄目前要assign值的位置
    // count 表示 目前相同的元素出現過幾次
    int pos = 0;
    int count = 0;
    for (int i = 0; i < nums.length; i++) {
        if (i > 0 && nums[i - 1] == nums[i]) {
            count++;
            if (count >= 3) {
                continue;
            }
        } else {
            // 遇到第一個元素或是遇到第一次相同的兩個元素，
            // 則 count 為 1
            count = 1;
        }
        
        // 每次皆把值assign給pos的位置，除非 count 大於 2
        nums[pos++] = nums[i];
    }
    
    return pos;
}
```

---

另一寫法
```java
public int removeDuplicates(int[] nums) {
    int len = nums.length;
    if (len <= 2) {
        return len;
    }
    int pre = 1;
    int cur = 1;
    int occur = 1;
    while (cur < len) {
        if (nums[cur] == nums[cur - 1]) {
            if (occur >= 2) {
                //超過規定數目，直接忽略目前值，往下一個前進
                //設continue 就是只往前，而不作assign的動作，且也防止out of bound問題
                cur++;
                continue;
            } else {
                //還沒超過規定數目，因此occur++
                occur++;
            }
        } else {
            //因不同，而重新計算，occur重設
            occur = 1;
        }
        //每輪不管如何，都把cur的值assign給pre
        nums[pre] = nums[cur];
        pre++;
        cur++;
    }
    return pre;
}
```
