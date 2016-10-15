# List Cycle

[Leetcode](https://leetcode.com/problems/linked-list-cycle/)

題意：

Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?


解題思路：

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null) {
            return false;
        }
        
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow.val == fast.val) {
                return true;
            }
           
        }
        return false;
    }
}
```