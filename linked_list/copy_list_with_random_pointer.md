# Copy List with Random Pointer



```java
public RandomListNode copyRandomList(RandomListNode head) {

    RandomListNode dummy = new RandomListNode(0);
    RandomListNode last = dummy, newNode;
    HashMap<RandomListNode, RandomListNode> map= new HashMap<RandomListNode, RandomListNode>();

    while ( head != null ) {
        if ( map.containsKey(head) ) {
            newNode = map.get(head);
        } else {
            newNode = new RandomListNode(head.label);
            map.put(head, newNode);
        }
        last.next = newNode;

        if ( head.random != null ) {
            if ( map.containsKey(head.random) ) {
                newNode = map.get(head.random);
            } else {
                newNode = new RandomListNode(head.random.label);
                map.put(head.random, newNode);
            }
            last.next.random = newNode;
        }
        last = last.next;
        head = head.next;
    }
    return dummy.next;
}
```

>Space Complexity:O(n)，因需要使用hashmap


Interleavin Solution(不需hashmap)

```java
public class InterleavingSolution {
    /**
     * @param head: The head of linked list with a random pointer.
     * @return: A new head of a deep copy of the list.
     */
    public RandomListNode copyRandomList(RandomListNode head) {
        if (head == null) {
            return null;
        }
        copyNext(head);
        copyRandom(head);
        return splitList(head);
    }

    //先把每個list中的node 複製一份新的node交插的合到原本的list裡
    //把原本的random也順便copy過去
    private void copyNext(RandomListNode head) {
        while (head != null) {
            RandomListNode newNode = new RandomListNode(head.label);
            newNode.random = head.random;
            newNode.next = head.next;
            head.next = newNode;
            head = head.next.next;
        }
    }
    //只要把新node的random指向舊node.random的下一個即可
    private void copyRandom(RandomListNode head) {
        while (head != null) {
            if (head.next.random != null) {
                head.next.random = head.random.next;
            }
            head = head.next.next;
        }
    }

    private RandomListNode splitList(RandomListNode head) {
        RandomListNode newHead = head.next;
        while (head != null) {
            //temp為新list的node，先取出來
            //接著改變head.next指回原本舊的node
            //移動舊head到下一個舊node
            //如果temp.next還有(目前指到舊node)，則指到temp的下下個(即為新node)
            RandomListNode temp = head.next;
            head.next = temp.next;
            head = head.next;
            if (temp.next != null) {
                temp.next = temp.next.next;
            }
        }
        return newHead;
    }
}
```
