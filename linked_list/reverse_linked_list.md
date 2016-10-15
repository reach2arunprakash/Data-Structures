# Reverse Linked List
[原題網址](http://www.lintcode.com/en/problem/reverse-linked-list/)

>Reverse a linked list.

題意：

反轉一個 Linked List

解題思路：

利用三個指針 prev， cur ， next來幫助我們完成這道題，操作步驟與程式碼如下：

     null                   1-->2-->3-->4-->null
     null<--1               2-->3-->4-->null
     null<--1<--2           3-->4-->null
     null<--1<--2<--3       4-->null
     null<--1<--2<--3<--4   null

```java
public ListNode reverse(ListNode head) {
    
    if (head == null || head.next == null) {
        return head;
    }

    // null             1-->2-->3-->4-->null
    // null<--1         2-->3-->4-->null
    // null<--1<--2     3-->4-->null
    // null<--1<--2<--3 4-->null
    // null<--1<--2<--3<--4 null
    //主要使用三個pointer，prev，cur，next
    //因cur.next改指針時會消失，得用next記著
    //去畫圖就能理解
    ListNode prev = null;
    ListNode cur = head;
    
    while (cur != null) {
        ListNode next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    return prev;
}
```

網上看到了一個不用改變指針的方式，先紀錄一下

```java

public class Solution {
     
    ListNode left = null;
    boolean meet = false;
    public ListNode reverse(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        left = head;
        meet = false;
        helper(head);
        return head;
    }
    
    public ListNode helper(ListNode head) {
        if (head.next == null) {
            return head;
        }
        
        ListNode right = helper(head.next);
        if (!meet) {
            int tmp = left.val;
            left.val = right.val;
            right.val = tmp;
            if (left == right || left.next == right) {
                meet = true;
            }
            
            left = left.next;
        }
        
        return head;
    }
}


```