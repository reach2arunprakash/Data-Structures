# Swap Nodes in Pairs

[Lintcode](http://www.lintcode.com/en/problem/swap-nodes-in-pairs/)

題意：

Given a linked list, swap every two adjacent nodes and return its head.

Example
```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

Challenge

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

解題思路：

因head可能會改變，因此用dummy node來紀錄一下，利用 四個指針分別記錄 prev, head, head next, next next, 接著再不斷的作改變指針的行為，最後返回 dummy.next，程式碼如下：

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    /**
     * @param head a ListNode
     * @return a ListNode
     */
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;
        while (head != null && head.next != null) {
            ListNode nextnext = head.next.next;
            prev.next = head.next;
            head.next.next = head;
            head.next = nextnext;
            prev = head;
            head = head.next;
        }
        return dummy.next;
    }
}

```