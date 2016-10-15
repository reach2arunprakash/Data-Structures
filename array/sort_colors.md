# Sort Colors
[原題網址](http://www.lintcode.com/en/problem/sort-colors/)

題意：陣列中只有三個顏色(0, 1, 2)，將此三個顏色排序

解題思路：

利用三個 pointer (left, right, point)，接著透過point以下三種情況作處理
left 代表0
point 代表1，並作indicator去走訪整個陣列
right 代表2

1. nums[point] == 0：表示需要往前移，因此跟left的值作交換，此時nums[left] 已確定是0，直接left與point往下移
2. nums[point] == 1：表示放在正確的位置，直接point往下移
3. nums[point] == 2：表示需要與right作交換，但由於當下的right也指向2，與point換了之後，point還是指向另一個二，因此只移動right。

```java
class Solution {

    public void sortColors(int[] nums) {
        if (nums == null || nums.length == 0) {
            return;
        }
        int left = 0;
        int right = nums.length - 1;
        int point = 0;
        // 利用三個指針，left, right, point
        // left負責0，point負責1，right負責2
        while (point <= right) {

            if (nums[point] == 0) {
                swap(nums, left, point);
                left++;
                point++;
            } else if (nums[point] == 1) {
                point++;
            } else {
                swap(nums, point, right);
                right--; //只移動right，因可能當下right指向的值也是2，要在下一輪繼續處理

            }
        }
    }

    private void swap(int[] A, int idxA, int idxB) {
        int temp = A[idxA];
        A[idxA] = A[idxB];
        A[idxB] = temp;
    }
}

```
