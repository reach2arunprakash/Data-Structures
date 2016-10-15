# Add Two Numbers

[Lintcode](http://www.lintcode.com/en/problem/add-two-numbers/)
[Leetcode](https://leetcode.com/problems/add-two-numbers/)

題意：

You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in **reverse** order, such that the 1's digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list.

Example
Given 7->1->6 + 5->9->2. That is, 617 + 295.

Return 2->1->9. That is 912.

Given 3->1->5 and 5->9->2, return 8->0->8.

解題思路：

updated on 2015.12.27

網友提供了簡潔程式碼：

```java
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode prev = new ListNode(0);
        ListNode head = prev;
        int carry = 0;
        while (l1 != null || l2 != null || carry != 0) {
            ListNode cur = new ListNode(0);
            int sum = ((l2 == null) ? 0 : l2.val) + ((l1 == null) ? 0 : l1.val) + carry;
            cur.val = sum % 10;
            carry = sum / 10;
            prev.next = cur;
            prev = cur;

            l1 = (l1 == null) ? l1 : l1.next;
            l2 = (l2 == null) ? l2 : l2.next;
        }
        return head.next;
    }
}
```

此為CC150的題，先用了非遞迴方式實作，程式碼長了點，程式碼如下：

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;      
 *     }
 * }
 */
public class Solution {
    /**
     * @param l1: the first list
     * @param l2: the second list
     * @return: the sum list of l1 and l2 
     */
    public ListNode addLists(ListNode l1, ListNode l2) {
        
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        int carry = 0;
        while (l1 != null && l2 != null) {
            int val = l1.val + l2.val + carry;
            carry = val / 10;
            val %= 10;
            ListNode cur = new ListNode(val);
            prev.next = cur;
            prev = prev.next;
            l1 = l1.next;
            l2 = l2.next;
        }
        
        while (l1 != null) {
            int val = l1.val + carry;
            carry = val / 10;
            val %= 10;
            ListNode cur = new ListNode(val);
            prev.next = cur;
            prev = prev.next;
            l1 = l1.next; 
        }
        
        while (l2 != null) {
            int val = l2.val + carry;
            carry = val / 10;
            val %= 10;
            ListNode cur = new ListNode(val);
            prev.next = cur;
            prev = prev.next;
            l2 = l2.next; 
        }
        
        if (carry > 0) {
            ListNode cur = new ListNode(carry);
            prev.next = cur;
            prev = prev.next;
        }
        
        return dummy.next;
    }
}
```

而 CC150 官方解答給了遞迴的方式，程式碼較簡潔，但需注意null pointer的問題

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;      
 *     }
 * }
 */
public class Solution {
    /**
     * @param l1: the first list
     * @param l2: the second list
     * @return: the sum list of l1 and l2 
     */
    public ListNode addLists(ListNode l1, ListNode l2) {
        if (l1 != null && l2 == null) {
            return l1;
        } else if (l1 == null && l2 != null) {
            return l2;
        } else {
            return helper(l1, l2, 0);
        }
    }
    
    public ListNode helper(ListNode l1, ListNode l2, int carry) {
        if (l1 == null && l2 == null && carry == 0) {
            return null;
        }
        
        ListNode cur = new ListNode(carry);
        if (l1 != null) {
            cur.val += l1.val;
        }
        if (l2 != null) {
            cur.val += l2.val;
        }
        
        carry = cur.val / 10;
        cur.val = cur.val % 10;
        
        if (l1 != null || l2 != null) {
            ListNode next = helper(l1 == null ? null : l1.next, l2 == null ? null : l2.next, carry);
            cur.next = next;
        } 
        
        
        
        return cur;
    }
}
```