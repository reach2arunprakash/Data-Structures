# Insertion Sort List

[Lintcode](http://www.lintcode.com/en/problem/insertion-sort-list/)

題意：

Sort a linked list using insertion sort.

Example

Given 1->3->2->0->null, return 0->1->2->3->null.

解題思路：

每次把head節點往前找到適合的位置往前插，再繼續head下一個節點，找位置時記得從頭開始找。

其程式碼如下：

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @return: The head of linked list.
     */
    public ListNode insertionSortList(ListNode head) {
        
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        
        while (head != null) {
            ListNode next = head.next;
            prev = dummy; // 每次都要從頭開始找，所以記得assign到dummy
            while (prev.next != null && prev.next.val <= head.val) {
                prev = prev.next;
            }
            // 把cur插到prev的下一個
            head.next = prev.next;
            prev.next = head;
            head = next;
        }
        return dummy.next;
    }
}

```
---
###Reference
1. http://blog.csdn.net/linhuanmars/article/details/21144553