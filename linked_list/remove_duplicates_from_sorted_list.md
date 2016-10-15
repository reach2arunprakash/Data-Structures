#Remove Duplicates from Sorted List

[原題網址](http://www.lintcode.com/en/problem/remove-duplicates-from-sorted-list/)

題意：即實作 Linked List 的 Unique 方法，重複的元素只留一個。

解題思路：不斷的拿當前的 node 去與下一個 node 來比
* 若一樣，則改變 node.next 指到 node.next.next ，
* 若不同，則直接移動 node 到下一個，

由於head並不會變，所以不需要dummy node來輔助，程式碼如下：

```java
public static ListNode deleteDuplicates(ListNode head) { 
    
    if (head == null) {
        return head;
    }
    
    ListNode node = head;
    while (node.next != null) {
        if (node.val == node.next.val) {
            node.next = node.next.next;
        } else {
            node = node.next;
        }
    }
    return head;
}
```

---
Reference：

1. http://www.jiuzhang.com/solutions/remove-duplicates-from-sorted-list/