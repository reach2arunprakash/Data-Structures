#Partition List

[原題網址](http://www.lintcode.com/en/problem/partition-list/)

題意：

給定一鏈表，與值 x，將鏈表中所有比 x 小的元素放在鏈表的左邊，比 x 大的元素放在右邊。

解題思路：

分別建兩個鏈表，分別存放比 x 小的元素與比 x 大的元素，最後將兩個鏈表接起來即可，原始碼如下：


>記得要把greater的下一個斷掉，否則會變成loop


```java

public ListNode partition(ListNode head, int x) {
        
    if (head == null) {
        return null;
    }
    
    ListNode dummyLesser = new ListNode(0);
    ListNode dummyGreater = new ListNode(0);
    ListNode curLesser = dummyLesser;
    ListNode curGreater = dummyGreater;
    
    while (head != null) {
        if (head.val < x) {
            curLesser.next = head;
            curLesser = curLesser.next;
        } else {
            curGreater.next = head;
            curGreater = curGreater.next;
        }
        head = head.next;
    }
    
    curGreater.next = null;
    curLesser.next = dummyGreater.next;
    return dummyLesser.next;
}
```
---
>Time Complexity：O(N)