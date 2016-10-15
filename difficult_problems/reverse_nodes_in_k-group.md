# Reverse Nodes in k-Group

[Lintcode](http://www.lintcode.com/en/problem/reverse-nodes-in-k-group/)

題意：

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed. Only constant memory is allowed.


Example

Given this linked list: 1->2->3->4->5

For k = 2, you should return: 2->1->4->3->5

For k = 3, you should return: 3->2->1->4->5

解題思路：

為[Reverse Linked List II]() 的follow up

這題很容易搞混，參考了網友的解說，主要有使用了兩個函數，reverse與原本的主函數，

* 改良過的reverse函數，傳入pre與next值，終止條件為遇到next節點，reverse函數則是不斷的把節點往前移。
* 主函數，與原本的反轉鏈表一樣需要dummy node與pre節點，但此時需要一個counter，當counter % k == 0 時，代表我們需要來反轉這段子鏈表了，我們傳入要反轉的子鏈表的前後節點pre與next，接著透過reverse函數來將中間的節點來作反轉。




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
    /**
     * @param head a ListNode
     * @param k an integer
     * @return a ListNode
     */
    public ListNode reverseKGroup(ListNode head, int k) {
        
        if (head == null || k == 1) {
            return head;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;
        int i = 0;
        while(head != null) {
            i++;
            // 當走到第k個元素時，開始反轉，
            // reverse函數會回傳pre.next 節點給 pre
            // 此時的pre是變成反轉後那一段的最後一個節點，
            // 此時再把head指到pre的下一個再開始計錄新的sub list
            if (i % k == 0) {
                pre = reverse(pre, head.next);
                head = pre.next;
            } else {
                head = head.next;
            }
        }
        
        return dummy.next;
    }
    
    // 把陣列反轉，不斷的把node放到pre的下一個
    // 終止條件為cur == next
    private ListNode reverse(ListNode pre, ListNode next) {
        ListNode last = pre.next;
        ListNode cur = last.next;
        while(cur != next) {
            last.next = cur.next;
            cur.next = pre.next;
            pre.next = cur;
            cur = last.next;
        }
        
        return last;
    }
}

```
---
###Reference
1. http://www.cnblogs.com/lichen782/p/leetcode_Reverse_Nodes_in_kGroup.html (解釋相當詳細)