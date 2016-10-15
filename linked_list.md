
# Linked List

什麼是Linked List


#### Single List
```java
// Definition for ListNode.
public class ListNode {
    int val;
    ListNode next;
    ListNode(int val) {
        this.val = val;
        this.next = null;
    }
}
```
#### Double List


How to implement Linked List data structure

- - - 
####LinkedList Template

當在處理 Linked List 的時候，最常寫下的片段如下：

```java
while ( head != null ) {
    // 自己的程式碼
    head = head.next;
}
```

這個片段就會讓程式從 Linked List 的頭節點走到最後一個節點 (null)，但切記別在 head 更新成下一個節點 head.next 之前，就擅自對 head 或是 head.next 進行改變。如果一定要對 head 或是 head.next做改變，可以利用一個暫時的 Linked List 節點存下 head.next ，最後再將 head 更新成暫時的節點，程式碼如下：

```java
while ( head != null ) {
    LinkedList temp = head.next;
    // 自己的程式碼
    
    head = temp;
}
```
這個用法我們將會在反轉 Linked List ([Reverse Linked List](./linked_list/reverse_linked_list.md)) 題目中見到。
- - -
common mistake in Linked List

LinkedList 特殊解法 ( Dummy Node, Fast-n-Slow pointer )

Linked List 題目
