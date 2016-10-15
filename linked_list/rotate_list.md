# Rotate List

[Lintcode](https://leetcode.com/problems/rotate-list/)

題意：

Given a list, rotate the list to the right by k places, where k is non-negative.

For example:

Given ```1->2->3->4->5->NULL``` and ```k = 2```,

return ```4->5->1->2->3->NULL```.

解題思路：

類似 Rotate Array一樣

先把整個 list 反轉，再把由第k個位置把list分成兩段，分別再reverse，最後再接起來即可，但這方法太多細節需要注意，還要處理 k 大於 length 的問題，也容易搞錯，程式碼如下 (目前過不了，待之後再修正)：

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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) {
            return head;
        }
        
        int len = 0;
        ListNode temp = head;
        while (temp != null) {
            len++;
            temp = temp.next;
        }
        
        k %= len;
        ListNode reversedHead = reverse(head);
        ListNode cur = reversedHead;
        ListNode prev = null;
        for (int i = 0; i < k; i++) {
            prev = cur;
            cur = cur.next;
        }
        
        ListNode nextK = cur;
        prev.next = null;
        
        ListNode firstHalf = reverse(reversedHead);
        ListNode secondHalf = reverse(nextK);
        
        cur.next = secondHalf;
        
        return firstHalf;
     }
    
    public ListNode reverse(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
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
}
```

網友 [水中的魚](http://fisherlei.blogspot.com/2013/01/leetcode-rotate-list.html) 提供了以下思路：

首先從head開始跑，直到最後一個節點，這時可以得出鏈表長度len。然後將尾指針指向頭指針，將整個圈連起來，接著往前跑len – k%len，從這裡斷開，就是要求的結果了。


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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) {
            return head;
        }
        
        //先把 list 接起來
        ListNode cur = head;
        int len = 1;
        while (cur.next != null) {
            cur = cur.next;
            len++;
        }
        cur.next = head;
        
        k = len - k % len;
        int step = 0;
        while (step < k) {
            cur = cur.next;
            step++;
        }
        head = cur.next;
        cur.next = null;
        return head;
    }
}
```

updated 2015.11.19，會忘，再紀錄一下。

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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || k == 0) {
            return head;
        }
        
        int len = 1;
        ListNode copyHead = head;
        while (copyHead.next != null) {
            copyHead = copyHead.next;
            len++;
        }
        
        copyHead.next = head;
        for (int i = 1; i < len - k % len; i++) {
            head = head.next;
        }
        
        copyHead = head.next;
        head.next = null;
        
        return copyHead;
        
    }
}
```

---
###Reference
1. http://fisherlei.blogspot.com/2013/01/leetcode-rotate-list.html