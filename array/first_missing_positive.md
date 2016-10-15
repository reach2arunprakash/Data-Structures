#First Missing Positive

[原題網址](http://www.lintcode.com/en/problem/first-missing-positive/)

題意：給定一個一維矩陣，找出第一個遺失的正整數。

Given an unsorted integer array, find the first missing positive integer.

For example,

Given ```[1,2,0]``` return 3,

and ```[3,4,-1,1]``` return 2.

Your algorithm should run in O(n) time and uses constant space.

解題思路：

1. 排序後再檢查，花O(NlogN)時間

2. 利用把A[i]放到A[A[i] - 1]的位置，最後再檢查A[i] 是否等於 i + 1，以 ```[3,4,-1,1]``` 為例

init : [3,4,-1,1]

i = 0 : A[0] = 3，3應該要在A[2]的位置，因此A[0] 與 A[2] 交換，目前A[0]為 -1 不需再檢查，往下一個前進。

[-1,4,3,1]


i = 1 : A[1] = 4，4應該要在A[3]的位置，因此A[1] 與 A[3] 交換，目前A[1] = 1, 由於 A[1] 非負數，需把1 放在對的位置 A[0] ，所以 A[0] 與 A[1] 交換，結果為：

[1,-1,3,4]


i = 2 : A[2] = 3，在對的位置，不用移。

i = 3 : A[3] = 4，在對的位置，不用移。

最後再掃一次處理後的陣列，找出第一A[i] != i + 1 值即可。

程式碼如下：

> 當作交換動作後，記得把i減回來再檢查一下交換到當前值是否該對的位置。

```java
public class Solution {
    public int firstMissingPositive(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 1;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0 && nums[i] <= nums.length) {
                if (nums[nums[i] - 1] != nums[i]) {
                    int temp = nums[nums[i] - 1];
                    nums[nums[i] - 1] = nums[i];
                    nums[i] = temp;
                    i--; // 換過之後，記得把i減回來再檢查一下交換到當前值是否該對的位置。
                }
            }
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        
        return nums.length + 1;
    }
}
```

---
###Reference
1. http://blog.csdn.net/kenden23/article/details/17099987
