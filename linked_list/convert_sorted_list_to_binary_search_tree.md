# Convert Sorted List to Binary Search Tree
[原題網址](http://www.lintcode.com/en/problem/convert-sorted-list-to-binary-search-tree/)


Definition for ListNode:
```
public class ListNode {
    int val;
    ListNode next;
    ListNode(int val) {
        this.val = val;
        this.next = null;
    }
}
```
Definition of TreeNode:
```
public class TreeNode {
    public int val;
    public TreeNode left, right;
    public TreeNode(int val) {
        this.val = val;
        this.left = this.right = null;
    }
}
```
```java
public TreeNode sortedListToBST(ListNode head) {
    if ( head == null ) {
        return null;
    }

    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode slow = dummy;
    ListNode fast = head;

    while ( fast != null && fast.next != null ) {
        slow = slow.next;
        fast = fast.next.next;
    }

    TreeNode root = new TreeNode(slow.next.val);

    TreeNode right = sortedListToBST(slow.next.next);
    slow.next = null;
    TreeNode left = sortedListToBST(dummy.next);

    root.right = right;
    root.left = left;

    return result;
}
```

```java

private ListNode current;

private int getListLength(ListNode head) {
    int size = 0;
    while (head != null) {
        size++;
        head = head.next;
    }
    return size;
}

public TreeNode sortedListToBST(ListNode head) {
    int size;

    current = head;
    size = getListLength(head);
    return sortedListToBSTHelper(size);
}

public TreeNode sortedListToBSTHelper(int size) {
    if (size <= 0) {
        return null;
    }

    TreeNode left = sortedListToBSTHelper(size / 2);
    TreeNode root = new TreeNode(current.val);
    current = current.next;
    TreeNode right = sortedListToBSTHelper(size - 1 - size / 2);

    root.left = left;
    root.right = right;

    return root;
}
```
