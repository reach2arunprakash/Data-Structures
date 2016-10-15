# Remove Nth Node From End of List
[原題網址](http://www.lintcode.com/en/problem/remove-nth-node-from-end-of-list)

Given a linked list, remove the nth node from the end of list and return its head.

Definition for ListNode.
```
public class ListNode {
    int val;
    ListNode next;
    ListNode(int val) {
        this.val = val;
        this.next = null;
    }
}

```


```
@param head: The first node of linked list.
@param n: An integer.
@return: The head of linked list.
```
## Solutions

```java
ListNode removeNthFromEnd(ListNode head, int n) {
    // write your code here

    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode slow = dummy;

    while ( head != null && n > 0 ) {
        head = head.next;
        n--;
    }
    if ( n > 0 ) {
        return null;
    }
    while ( head != null ) {
        slow = slow.next;
        head = head.next;
    }
    slow.next = slow.next.next;
    return dummy.next;
}
```
第二種解法
```java
ListNode removeNthFromEnd(ListNode head, int n) {

    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode target = dummy;

    for ( int i = 0 ; i < n ; i ++ ) {
        if ( head == null ) {
            return null;
        }
        head = head.next;
    }

    while ( head != null ) {
        target = target.next;
        head = head.next;
    }
    target.next = target.next.next;
    return dummy.next;
}
```
