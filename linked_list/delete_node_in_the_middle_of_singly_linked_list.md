#Delete Node in the Middle of Singly Linked List

[原題網址](http://www.lintcode.com/en/problem/delete-node-in-the-middle-of-singly-linked-list/)

題意：

刪除鏈結中的第m個節點，且當下只能存取該節點，無法得知之前的節點是什麼。

解題思路：

把下一點的值複製到當前節點，再把下一節點刪除即可，代碼如下：

```java
public void deleteNode(ListNode node) {
        
    if (node == null || node.next == null) {
        return;
    }
    
    node.val = node.next.val;
    node.next = node.next.next;
}
```