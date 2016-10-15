# Reorder List

[Lintcode](http://www.lintcode.com/en/problem/reorder-list/)

[Leetcode](https://leetcode.com/problems/reorder-list/)

題意：

Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You must do this in-place without altering the nodes' values.

For example,
Given {1,2,3,4}, reorder it to {1,4,2,3}.

解題思路：

這題考了多個 Linked List 的重要觀念，反轉，合併，找中間值，主要使用以下三個步驟來完成這道題。

1. 用快慢指針找到中間點
2. 把 list 從中間切開成兩個 list
3. 把後面那個 list作反轉的動作
4. 合併兩個 list

其程式碼如下：

```java
public class Solution {
    /**
     * @param head: The head of linked list.
     * @return: void
     */
    public void reorderList(ListNode head) {  
        if (head == null || head.next == null) {
            return;
        }
        ListNode mid = findMid(head);
        ListNode right = reverse(mid.next);
        mid.next = null;
        merge(head, right);
    }
    
    private ListNode findMid(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode fast = head.next;
        ListNode slow = head;
        
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
    
    private void merge(ListNode nodeOne, ListNode nodeTwo) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        int num = 0;
        
        while(nodeOne != null && nodeTwo != null) {
            if (num % 2 == 0) {
                tail.next = nodeOne;
                nodeOne = nodeOne.next;
            } else {
                tail.next = nodeTwo;
                nodeTwo = nodeTwo.next;
            }
            tail = tail.next;
            num++;
        }
        if (nodeOne != null) {
            tail.next = nodeOne;
        } else {
            tail.next = nodeTwo;
        }
    }
    
    private ListNode reverse(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        
        
        while (current != null) {
            ListNode next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }
        return prev;
    }
}
```

updated 2015.10.7

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
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) {
            return;
        }
        
        ListNode list1 = head;
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode pre = slow;
        slow = slow.next;
        pre.next = null;
        
        ListNode list2 = reverse(slow);
        
        while (list1 != null && list2 != null) {
            ListNode list1Next = list1.next;
            ListNode list2Next = list2.next;
            list1.next = list2;
            list2.next = list1Next;
            list1 = list1Next;
            list2 = list2Next;
        }
        
        
    }
    
    private ListNode reverse(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode prev = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = prev;
            prev = head;
            head = next;
        }
        
        return prev;
        
    }
}
```