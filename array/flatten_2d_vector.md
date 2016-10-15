# Flatten 2D Vector

[Leetcode](https://leetcode.com/problems/flatten-2d-vector/)

題意：

Implement an iterator to flatten a 2d vector.

For example,
Given 2d vector =

[
  [1,2],
  [3],
  [4,5,6]
]
By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,2,3,4,5,6].


解題思路：


不用 java內建的 iterator，程式碼如下：

```java
public class Vector2D {
    Queue<Integer> q = new LinkedList<Integer>();
    public Vector2D(List<List<Integer>> vec2d) {
        for (List<Integer> list : vec2d) {
            for (int item : list) {
                q.offer(item);
            }
        }
    }

    public int next() {
        return q.poll();
    }

    public boolean hasNext() {
        return !q.isEmpty();
    }
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D i = new Vector2D(vec2d);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

Follow up : As an added challenge, try to code it using only iterators in C++ or iterators in Java.

```java
public class Vector2D {
    Queue<Iterator<Integer>> q = new LinkedList<Iterator<Integer>>();
    Iterator<Integer> current = null;
    public Vector2D(List<List<Integer>> vec2d) {
        for (List<Integer> list : vec2d) {
            q.offer(list.iterator());
        }
        current = q.poll();
    }

    public int next() {
        if (!current.hasNext()) {
            return -1;
        }
        return current.next();
    }

    public boolean hasNext() {
        if (current == null) {
            return false;
        }
        
        while (!current.hasNext()) {
            if (!q.isEmpty()) {
                current = q.poll();
            } else {
                return false;
            }
        }
        
        return true;
    }
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D i = new Vector2D(vec2d);
 * while (i.hasNext()) v[f()] = i.next();
 */
```


---
###Reference
1. https://leetcode.com/discuss/57984/simple-and-short-java-solution-with-iterator