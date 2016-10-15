#Merge Two Sorted Lists

[原題網址](http://www.lintcode.com/en/problem/merge-two-sorted-lists/)

題意：合併兩個有序鏈表

解題思路：

代碼如下：

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        
    if (l1 == null) {
        return l2;
    }
    
    if (l2 == null) {
        return l1;
    }
    
    ListNode dummy = new ListNode(0);
    ListNode cur = dummy;
    
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            cur.next = l1;
            l1 = l1.next;
        } else {
            cur.next = l2;
            l2 = l2.next;
        }
        cur = cur.next;
    }
    if (l1 == null) {
        cur.next = l2;
    } else {
        cur.next = l1;
    }
    return dummy.next;
}
```
