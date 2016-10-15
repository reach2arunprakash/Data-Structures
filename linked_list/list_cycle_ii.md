#List Cycle II

[原題網址](http://www.lintcode.com/en/problem/linked-list-cycle-ii/)

```java
public ListNode detectCycle(ListNode head) {  
    if (head == null || head.next == null) {
        return null;
    }
    
    // 使用快慢指針來作這題，如果兩者有相遇的話，必定有cycle，
    // 一但相同的話，改為相同速度來走，若遇到一樣，則該點為cycle的起始點
    ListNode slow = head;
    ListNode fast = head.next;
    
    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return null;
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // 需要找到為何是檢查slow.next
    while (slow.next != head) {
        slow = slow.next;
        head = head.next;
    }
    
    return head;
}
```


updated 2015.11.24

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
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                slow = head;
                while (fast != slow) {
                    fast = fast.next;
                    slow = slow.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```