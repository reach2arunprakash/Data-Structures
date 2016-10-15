#Count of Smaller Number

[原題連結](http://www.lintcode.com/en/problem/count-of-smaller-number/)

題意：給予兩個陣列 A 與 Queries，根據 Queries 陣列中的每個元素，去找出 A 中比 Queries[i] 小的元素個素有幾個。

解法：此題可有三個解法，
1. 兩層循環，複雜度高
2. 排序後再一次遍歷
3. 線段樹

## 兩層循環解法

```java
public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
    
    ArrayList<Integer> res = new ArrayList<Integer>();
    
    if (A == null ||  queries == null || queries.length == 0) {
        return res;
    }
    
    for (int i = 0; i < queries.length; i++) {
        int count = 0;
        for (int j = 0; j < A.length; j++) {
            if (queries[i] > A[j]) {
                count++;
            }
        }
        res.add(count);
    }
    
    return res;
}
```

>時間複雜度：O(n^2)

1. Just loop

```java
public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
    ArrayList<Integer> result = new ArrayList<Integer>();
    if ( queries == null || queries.length == 0 ) {
        return result;
    }
    
    for ( int query : queries ) {
        int count = 0;
        for ( int j = 0 ; j < A.length ; j++ ) {
            if ( A[j] < query ) {
                count++;
            }
        }
        result.add(count);
    }
    return result;
}
```
2. Sort and Binary

```java
public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
    ArrayList<Integer> result = new ArrayList<Integer>();
    if ( queries == null || queries.length == 0 ) {
        return result;
    }
    
    Arrays.sort(A);
    for ( int query : queries ) {
        if ( A == null || A.length == 0 ) {
            result.add(0);
            continue;
        }
        int start = 0;
        int end = A.length-1;
        while ( start + 1 < end ) {
            int mid = start + ( end - start ) / 2;
            if ( A[mid] < query ) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if ( A[start] >= query ) {
            result.add(start);
        } else if ( A[end] >= query ) {
            result.add(end);
        } else {
            result.add(A.length);
        }
    }
    return result;
}
```
3. Segment Tree Solution

```java
public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
    ArrayList<Integer> result = new ArrayList<Integer>();
    if ( queries == null || queries.length == 0 ) {
        return result;
    }
    
    SegmentTreeNode root = build(0, 100000);
    for ( int num : A ) {
        modify(root, num);
    }
    for ( int q : queries ) {
        if ( q >= 0 ) {
            result.add(query(root, 0, q-1));
        } else {
            result.add(0);
        }
    }
    return result;
}

public class SegmentTreeNode {
    public int start, end, count;
    public SegmentTreeNode left, right;
    public SegmentTreeNode(int start, int end, int count) {
        this.start = start;
        this.end = end;
        this.count = count;
        this.left = null;
        this.right = null;
    }
}

public SegmentTreeNode build(int start, int end) {
    if ( start > end ) {
        return null;
    }
    SegmentTreeNode root = new SegmentTreeNode(start, end, 0);
    if ( start != end ) {
        int mid = start + ( end - start ) / 2;
        root.left = build(start, mid);
        root.right = build(mid+1, end);
        root.count = root.left.count + root.right.count;
    }
    return root;
}

public void modify(SegmentTreeNode root, int value) {
    // if ( value < root.start || root.end < value ) {
    //     return;
    // }
    if ( root.start == value && root.end == value ) {
        root.count++;
        return;
    }
    int mid = root.start + ( root.end - root.start ) / 2;
    if ( value <= mid ) {
        modify(root.left, value);
    } else {
        modify(root.right, value);
    }
    root.count = root.left.count + root.right.count;
    return;
}

public int query(SegmentTreeNode root, int start, int end) {
    if ( root.start == start && root.end == end ) {
        return root.count;
    }
    int left = 0 ; int right = 0;
    int mid = root.start + ( root.end - root.start ) / 2;
    if ( start <= mid ) {
        if ( mid < end ) {
            left = query(root.left, start, mid);
        } else {
            left = query(root.left, start, end);
        }
    }
    if ( mid < end ) {
        if ( start <= mid ) {
            right = query(root.right, mid+1, end);
        } else {
            right = query(root.right, start, end);
        }
    }
    return left + right;
}
```
