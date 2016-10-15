#Reverse Linked List II

[原題網址](http://www.lintcode.com/en/problem/reverse-linked-list-ii/)

題意：

為 [Reverse Linked List](linked_list/reverse_linked_list.md) 的 follow up ，給定一鏈表，與兩常數 m 與 n ，分別代表需要翻連鏈表中的開頭到結尾。

解題思路：

等於把鏈表切成三段 (前，中，後)，接著反轉中間那段，最後再將中段與前段與後段接起來，除了反轉之外，我們需要記住四個點：

* preM ：起始點M的前面那點
* mNode：第 m 點
* postN：第 n 點的後面那點
* nNode：第 n 點

接著依照 [Reverse Linked List](linked_list/reverse_linked_list.md) 的方法來作反轉，最後使用下面步驟把反轉那鏈表接回去

```java
preM.next = nNode;
mNode.next = postN;
```

其原始碼如下：

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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (head == null || head.next == null || m == n) {
            return head;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        head = dummy;
        
        for (int i = 1; i < m; i++) {
            if (head == null) {
                return null;
            }
            head = head.next;
        }
        
        ListNode preM = head;
        ListNode mNode = head.next;
        ListNode nNode = mNode;
        ListNode postN = mNode.next;
        for (int i = m; i < n; i++) {
            
            if (postN == null) {
                return null;
            }
            ListNode next = postN.next;
            postN.next = nNode;
            nNode = postN;
            postN = next;
        }
        
        preM.next = nNode;
        mNode.next = postN;
        return dummy.next;
        
    }
}
```

---
###Reference
1. http://www.jiuzhang.com/solutions/reverse-linked-list-ii/