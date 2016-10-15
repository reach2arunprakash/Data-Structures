# LRU Cache

[Lintcode](http://www.lintcode.com/en/problem/lru-cache/)

題意：

Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: ```get``` and ```set```.

```get(key)``` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.

```set(key, value)``` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

解題思路：

updated on 2016.1.10

java有個新的類叫LinkedHashMap可以使用，另外還可以override他原本的removeEldestElement方法，程式碼如下：

```java
import java.util.*;

public class LRUCache {
    private int capacity;
    private Map<Integer, Integer> map;

    public LRUCache(int c) {
      this.capacity = c;
      this.map = new LinkedHashMap<Integer, Integer>(capacity, 0.75f, true) {
        protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
            return size() > capacity;
        }
      };
    }

    public int get(int key) {
        if (!map.containsKey(key)) {
            return -1;
        }
        return map.get(key);
    }

    public void set(int key, int value) {
        map.put(key, value);
    }
}
```

```java
import java.util.*;

public class LRUCache {
    LinkedHashMap<Integer,Integer> map;
    int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new LinkedHashMap<Integer,Integer>(capacity + 1);
    }

    public int get(int key) {
        Integer val = map.get(key);
        if(val == null) {
            return -1;
        } else {
            map.remove(key); // reorder
            map.put(key, val);
            return val.intValue();
        }
    }

    public void set(int key, int value) {
        map.remove(key); // reorder
        map.put(key, value);
        if(map.size() > capacity) {
            map.remove(map.entrySet().iterator().next().getKey());
        }
    }
}
```

首先要知道LRU(Least Recently Used) Cache是什麼，由於cache是memory的子集，cache會存放最近使用的page，當cache滿的時候需要讀入新的page時，他會將最不常使用的page移出cache才有空間讀入新的page。

我們可以使用一個雙向鍊表(Double Linked List)來幫助我們，因為我們常常要從尾部把頁面移除，從前面加入新頁面。

加上HashMap幫助我們快速訂位該node在哪裡，一但知道node在哪裡，刪除node或移動node到頭只需要 O(1) 的時間複雜度。

其程式碼如下：


```java
public class Solution {
    
    //使用一個雙向鏈表，並設定key，val，因set func需要key value
    public class Node {
        int key;
        int val;
        Node prev;
        Node next;
        
        public Node(int key, int val) {
            this.key = key;
            this.val = val;
            this.prev = null;
            this.next = null;
        }
    }
    
    // @param capacity, an integer
    // 使用capacity來計錄cache的大小
    // 使用頭尾兩個node來紀錄前後
    // 使用HashMap當作cache，並直接紀錄該點在鏈表的哪個位置
    // 使用雙向鏈表是為了刪減移動方便
    private int capacity;
    private Node head = new Node(-1, -1);
    private Node tail = new Node(-1, -1);
    HashMap<Integer, Node> map = new HashMap<Integer, Node>();
    public Solution(int capacity) {
        this.capacity = capacity;
        tail.prev = head;
        head.next = tail;
    }

    // @return an integer
    public int get(int key) {
        //若cache裡找不到則返回-1
        if (!map.containsKey(key)) {
            return -1;
        }
        // 若有的話，則直接透過map把該點抓出來
        // 並改變該node前後node的pointer references
        // 再把該node透過func移到尾端
        Node cur = map.get(key);
        cur.prev.next = cur.next;
        cur.next.prev = cur.prev;
        moveToTail(cur);
        
        return cur.val;
    }

    // @param key, an integer
    // @param value, an integer
    // @return nothing
    public void set(int key, int value) {
        //若找得到該node，則直接改該node的值
        if (get(key) != -1) {
            map.get(key).val = value;
            return;
        }
        
        //若超出的話，則把最前面那個node從cache中刪除
        //並且改變head與該點下一個的pointer references
        if (map.size() == capacity) {
            map.remove(head.next.key);
            head.next = head.next.next;
            head.next.prev = head;
        }
        // 建一個新node並存到cache中，再透過func移到list最後面
        Node insert = new Node(key, value);
        map.put(key, insert);
        moveToTail(insert);
    }
    
    // 把current移到尾巴
    // 因前面get function已處理了current前後點的pointer references
    // 在這裡只需要處理current，tail與tail.prev的pointer references
    public void moveToTail(Node current) {
        current.prev = tail.prev;
        tail.prev.next = current;
        tail.prev = current;
        current.next = tail;
        
    }
}

```

