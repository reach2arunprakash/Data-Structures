# Remove Linked List Elements

[Lintcode](http://www.lintcode.com/en/problem/remove-linked-list-elements/)

題意：

Remove all elements from a linked list of integers that have value val.

Example
```
Given 1->2->3->3->4->5->3, val = 3, you should return the list as 1->2->4->5
```

解題思路：

updated on 2016.1.8

**Recursive:**
```java
public class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return null;
        }
        
        head.next = removeElements(head.next, val);
        
        return head.val == val ? head.next : head;
    }
}
```

**Iterative:**

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
     * @param val an integer
     * @return a ListNode
     */
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;
        while (head != null) {
            if (head.val != val) {
                prev.next = head;
                prev = prev.next;
            }
            head = head.next;
        }
        // 記得作這個動作，否則prev可能還指向等於val的節點
        prev.next = head;
        return dummy.next;
    }
}

```