# Intersection of Two Linked Lists

[Leetcode](https://leetcode.com/problems/intersection-of-two-linked-lists/)

題意：

Write a program to find the node at which the intersection of two singly linked lists begins.


For example, the following two linked lists:
```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```
begin to intersect at node c1.


Notes:

If the two linked lists have no intersection at all, return null.
The linked lists must retain their original structure after the function returns.
You may assume there are no cycles anywhere in the entire linked structure.
Your code should preferably run in O(n) time and use only O(1) memory.



解題思路：


先找出兩條 list 的長度，接著長的那條先走lenA - lenB步，接著較短的那條也跟著一起走直到走到底或是找到交插口。

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
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        
        if (headA == null || headB == null) {
            return null;
        }
        int lenA = 0;
        int lenB = 0;
        ListNode curA = headA;
        ListNode curB = headB;
        
        while (curA != null) {
            lenA++;
            curA = curA.next;
        }
        
        while (curB != null) {
            lenB++;
            curB = curB.next;
        }
        
        if (lenB > lenA) {
            ListNode temp = headA;
            headA = headB;
            headB = temp;
        }
        
        for (int i = 0; i < Math.abs(lenA - lenB); i++) {
            headA = headA.next;
        }
        
        while(headA != null && headB != null) {
            if (headA == headB) {
                return headA;
            }
            headA = headA.next;
            headB = headB.next;
        }
        
        return null;
    }
}
```

神之解法，由leetcode網友提供

"Two pointer solution (O(n+m) running time, O(1) memory):
Maintain two pointers pA and pB initialized at the head of A and B, respectively. Then let them both traverse through the lists, one node at a time.
When pA reaches the end of a list, then redirect it to the head of B (yes, B, that's right.); similarly when pB reaches the end of a list, redirect it the head of A.
If at any point pA meets pB, then pA/pB is the intersection node.
To see why the above trick would work, consider the following two lists: A = {1,3,5,7,9,11} and B = {2,4,9,11}, which are intersected at node '9'. Since B.length (=4) < A.length (=6), pB would reach the end of the merged list first, because pB traverses exactly 2 nodes less than pA does. By redirecting pB to head A, and pA to head B, we now ask pB to travel exactly 2 more nodes than pA would. So in the second iteration, they are guaranteed to reach the intersection node at the same time.
If two lists have intersection, then their last nodes must be the same one. So when pA/pB reaches the end of a list, record the last element of A/B respectively. If the two last elements are not the same one, then the two lists have no intersections.
"

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
        
        ListNode pA = headA;
        ListNode pB = headB;
        
        ListNode tailA = null;
        ListNode tailB = null;
        
        while (true) {
            if (pA == null) {
                pA = headB;
            }
            
            if (pB == null) {
                pB = headA;
            }
            
            if (pA.next == null) {
                tailA = pA;
            }
            
            if (pB.next == null) {
                tailB = pB;
            }
            
            //The two links have different tails. So just return null;
            if (tailA != null && tailB != null && tailA != tailB) {
                return null;
            }
            
            if (pA == pB) {
                return pA;
            }
            
            pA = pA.next;
            pB = pB.next;
        }
    }
}
```

---
###Reference
1. http://www.cnblogs.com/yuzhangcmu/p/4128794.html