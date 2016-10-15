# Peeking Iterator

[Leetcode](https://leetcode.com/problems/peeking-iterator/)

題意：

網友[書影](http://bookshadow.com/weblog/2015/09/21/leetcode-peeking-iterator/) 提供的題目大意：

給定一個迭代器類接口，包含方法: next()和hasNext()，設計並實現一個PeekingIterator支持peek()操作 -- 其本質就是把原本應由next()方法返回的元素peek()出來。

這裡有一個例子。假設迭代器初始化為list：[1, 2, 3]。

調用next()得到1，列表的第一個元素。

現在調用peek()然後返回2，下一個元素。在此之後調用next()仍然返回2。

最後一次調用next()返回3，末尾元素。此後調用hasNext()應該返回false。

提示：

考慮"預先獲取"。你可能需要緩存下一個元素。
一個變量夠用嗎？原因是什麼？
通過在next()之前調用peek()，以及在peek()之前調用next()測試你的設計。
關於本問題的一個清晰的實現，可以參考Google的guava庫源代碼。

進一步思考：怎樣拓展你的設計？使之變得通用化，適應所有的類型，而不只是整數？



題解思路：

網友 [書影](http://bookshadow.com/weblog/2015/09/21/leetcode-peeking-iterator/) 提供的解題思路：

引入兩個額外的變量nextElement和peekFlag

nextElement標識peek操作預先獲取的下一個元素，peekFlag記錄當前是否已經執行過peek操作


```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html
class PeekingIterator implements Iterator<Integer> {
    
    private Iterator<Integer> iterator;
    private boolean peekFlag = false; // 標示是否已經執行過peek了
    private Integer nextElement = null; // 預先抓下一個
    
	public PeekingIterator(Iterator<Integer> iterator) {
	    this.iterator = iterator;
	}

    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        if (!peekFlag) {
            nextElement = iterator.next();
            peekFlag = true;
        }
        return nextElement;
	}

	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
	    if (!peekFlag) {
	        return iterator.next();
	    }
	    peekFlag = false;
	    Integer res = nextElement;
	    nextElement = null;
	    return res;
	}

	@Override
	public boolean hasNext() {
	    return peekFlag || iterator.hasNext();
	}
}
```

update on 2015.12.5

```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html
class PeekingIterator implements Iterator<Integer> {
    
    private Iterator<Integer> iterator;
    private Integer nextCachedValue = null;
	public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
	    this.iterator = iterator;
	    next();
	}

    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        return nextCachedValue;
	}

	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
	    Integer res = nextCachedValue;
	    nextCachedValue = iterator.hasNext() ? iterator.next() : null;
	    return res;
	    
	}

	@Override
	public boolean hasNext() {
	    return (nextCachedValue != null);
	}
}
```

---
###Reference
1. http://bookshadow.com/weblog/2015/09/21/leetcode-peeking-iterator/
2. https://leetcode.com/discuss/66069/simple-java-solution-using-cached-value-107ms