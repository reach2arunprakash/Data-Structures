# Palindrome Linked List

[Lintcode](http://www.lintcode.com/en/problem/palindrome-linked-list/)

題意：

Implement a function to check if a linked list is a palindrome.

解題思路：

updated 2015.12.1

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
    
    // 解法，先用快慢指針找到中間點，
    // 接著由頭與中間開始比，若有不對，直接反回false，
    // 若跑完了，則返回true
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode mid = reverse(slow);
        while (head != null) {
            if (mid.val != head.val) {
                return false;
            }
            mid = mid.next;
            head = head.next;
        }
        return true;

    }
    //反轉列表
    private ListNode reverse(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        while (cur != null) {
            ListNode temp = cur.next;
            cur.next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
}
```

法一：使用 stack，先用快慢指針走到中點，慢指針邊走邊加到stack，到中間之後，再不斷的把 stack 中的值 pop出來作比較，若不一樣則返回 false，如果 slow走到底之後，表示為 palindrome，返回 true。

>使用 head.next 是否等於null來判斷是奇數個還是偶數個，若為 null表示為奇數個，需要把 stack最上面的元素(即list正中間元素) pop掉。

此法需花費 O(N) 的空間複雜度。

程式碼如下：

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
     * @return a boolean
     */
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }
        
        Stack<Integer> s = new Stack<Integer>();
        ListNode slow = head;
        ListNode fast = head;
        s.push(slow.val); // 記得先塞值進去
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            s.push(slow.val);
        }
        
        // 表示個數為奇數，把中間值丟掉
        if (fast.next == null) {
            s.pop();
        }
        
        while (slow.next != null) {
            slow = slow.next;
            int tmp = s.peek();
            s.pop();
            if (slow.val != tmp) {
                return false;
            }
        }

        return true;
    }
}
```

法二：把 list切成兩半，把後半段反轉，接著再一一比較，時間複雜度為O(1)

以下參考九章算法

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
     * @return a boolean
     */
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }
        
        ListNode middle = findMiddle(head);
        middle.next = reverse(middle.next);
        
        ListNode p1 = head;
        ListNode p2 = middle.next;
        while (p1 != null && p2 != null && p1.val == p2.val) {
            p1 = p1.next;
            p2 = p2.next;
        }
        
        return p2 == null;
    }
    
    private ListNode findMiddle(ListNode head) {
        if (head == null) {
            return null;
        }
        
        ListNode fast = head.next;
        ListNode slow = head;
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
    }
    
    private ListNode reverse(ListNode head) {
        ListNode prev = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }
}

```

---
###Reference

1. http://www.cnblogs.com/grandyang/p/4635425.html
2. http://www.jiuzhang.com/solutions/palindrome-linked-list/
