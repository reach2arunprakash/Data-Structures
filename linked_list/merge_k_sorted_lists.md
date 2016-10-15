#Merge K Sorted Lists

[Lintcode](http://www.lintcode.com/en/problem/merge-k-sorted-lists/)

題意：

為[Merge 2 Sorted Lists](linked_list/merge_2_sorted_lists.md) 的 follow up，在此需要合併 k 個 list。

解題思路：

1. Divide and Conquer 法：即類似 merge sort的合併程序，不斷把lists分成左右兩邊分別合併，最後再將處理完的結果合併，其程式碼如下：

```java
public class  DPSolution {
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */

    // 在此用dc解法，不斷把k 個lists對半切處理，直到剩下兩個list，再call merge 2 lists的方法。
    public ListNode mergeKLists(List<ListNode> lists) {  
        // write your code here
        if (lists == null || lists.size() == 0) {
            return null;
        }
        return mergeHelper(lists, 0, lists.size() - 1);
    }

    // 跟原本merge 2 lists的差別只在於這個對切的方法而已。
    private ListNode mergeHelper(List<ListNode> lists, int start, int end) {
        if (start == end) {
            return lists.get(start);
        }
        int middle = start + (end - start) / 2;
        ListNode left = mergeHelper(lists, start, middle);
        ListNode right = mergeHelper(lists, middle+1, end);
        return mergeTwoLists(left, right);
    }
    
    private ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        
        while(list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                tail.next = list1;
                list1 = list1.next;
            } else {
                tail.next = list2;
                list2 = list2.next;
            }
            tail = tail.next;
        }
        
        if (list1 != null) {
            tail.next = list1;
        } else {
            tail.next = list2;
        }
        
        return dummy.next;
    }    
}

```
---

2. Heap 法：


```java
public ListNode mergeKLists(ArrayList<ListNode> lists) {
    PriorityQueue<ListNode> heap = new PriorityQueue<ListNode>(10,new Comparator<ListNode>(){
            @Override
            public int compare(ListNode n1, ListNode n2)
            {
                return n1.val-n2.val;
            }
        });
    for(int i=0;i<lists.size();i++)
    {
        ListNode node = lists.get(i); 
        if(node!=null)
        {
            heap.offer(node);
        }
    }
    ListNode head = null;
    ListNode pre = head;
    while(heap.size()>0)
    {
        ListNode cur = heap.poll();
        if(head == null)
        {
            head = cur;
            pre = head;
        }
        else
        {
            pre.next = cur;
        }
        pre = cur;
        if(cur.next!=null)
            heap.offer(cur.next);
    }
    return head;
}
```

