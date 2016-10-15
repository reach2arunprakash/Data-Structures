#Heapify

[原題連結](http://www.lintcode.com/en/problem/heapify/)

題意：把任何一陣列，經過heapify後，使得陣列滿足堆(heap)的條件，即根節點必定小於或大於所有左右子樹的節點。

解題思路：因葉節點無法作sift down，從最小的非葉子節點開始，從後面往前一次作sift down操作，不斷的確認下層保持堆的特性。


對於一個有n節點的樹，其葉節點至少有n/2 -1個。

```java
public class Solution {
    /**
     * @param A: Given an integer array
     * @return: void
     */
    public void heapify(int[] A) {
        for (int i = A.length / 2; i >= 0; i--) {
            siftdown(A, i);
        }
    }
    
    private void siftdown(int[] A, int k) {
        int smallest = k;
        int len = A.length;
        //k代表是root，接著不斷拿root去與左右子樹的root比，若子樹的值較小，
        //則與k交換，接著k指向到被交換的點，就是不斷去追縱一開始的ROOT
        //請注意，比的是值，smallest紀錄的是當前最小值的index
        while ( k < len) {
            if (k * 2 + 1 < len && A[k * 2 + 1] < A[smallest]) {
                smallest = k * 2 + 1;
            }
            if (k * 2 + 2 < len && A[k * 2 + 2] < A[smallest]) {
                smallest = k * 2 + 2;
            }
            if (smallest == k) {
                break;
            }
            
            int temp = A[k];
            A[k] = A[smallest];
            A[smallest] = temp;
            k = smallest;
        }
    }
}
```