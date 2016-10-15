
# Data Structures and Algorithms

#[www.guvi.in]



Very useful to understand the basics and to write better code.

#How thing scale

In one sentence: As the size of your job goes up, how much longer does it take to complete it?
# Big O Notation

```
Big O is useful to compare how well two algorithms will scale up as the number of inputs is 
increased. Big O describes the fundamental scaling nature of an algorithm.

```

#### Why is this so important?

```

Because software deals with problems that may differ in size by factors up to a trillion.

It is very difficult to measure the speed of software programs, and when we try, the answers 
can be very complex and filled with exceptions and special cases. This is a big problem, because 
all those exceptions and special cases are distracting and unhelpful when we want to compare two
 different programs with one another to find out which is "fastest".

Consider the canonical sorting example. Bubble-sort is O(n2) while merge-sort is O(n log n). 
Let's say you have two sorting applications, 

application A which uses bubble-sort and 

application B which uses merge-sort, and 

let's say that for input sizes of around 30 elements application A is 1,000x faster than 
application B at sorting. If you never have to sort much more than 30 elements then it's
 obvious that you should prefer application A, as it is much faster at these input sizes.
 However, if you find that you may have to sort ten million items then what you'd expect 
 is that application B actually ends up being thousands of times faster than application 
 A in this case, entirely due to the way each algorithm scales.

```

#### Constant — statement (one line of code)

```java
a += 1;
```

Growth Rate: **1**

#### Logarithmic — divide in half (binary search)

```java
while (n > 1) {
  n = n / 2;
}
```

Growth Rate: **log(n)**

#### Linear — loop

```java
for (int i = 0; i < n; i++) {
  // statements
  a += 1;
}
```

Growth Rate: **n**

The loop executes `N` times, so the sequence of statements also executes `N` times. If we assume the statements are `O(1)`, the total time for the for loop is `N * O(1)`, which is `O(N)` overall.

#### Quadratic — Effective sorting algorithms

```
Mergesort, Quicksort, …
```

Growth Rate: **n*log(n)**

#### Quadratic — double loop (nested loops)

```java
for (int c = 0; c < n; c++) {
  for (int i = 0; i < n; i++) {
    // sequence of statements
    a += 1;
  }
}
```

Growth Rate: **n^2**

The outer loop executes N times. Every time the outer loop executes, the inner loop executes `M` times. As a result, the statements in the inner loop execute a total of `N * M` times. Thus, the complexity is `O(N * M)`.
In a common special case where the stopping condition of the inner loop is `J < N` instead of `J < M` (i.e., the inner loop also executes `N` times), the total complexity for the two loops is `O(N2)`.

#### Cubic — triple loop

```java
for (c = 0; c < n; c++) {
  for (i = 0; i < n; i++) {
    for (x = 0; x < n; x++) {
      a += 1;
    }
  }
}
```

Growth Rate: **n^3**

#### Exponential — exhaustive search

```
Trying to break a password generating all possible combinations
```

Growth Rate: **2^n**

##### If-Then-Else

``` java
if (cond) {
  block 1 (sequence of statements)
} else {
  block 2 (sequence of statements)
}
```

If `block 1` takes `O(1)` and `block 2` takes `O(N)`, the `if-then-else` statement would be `O(N)`.

##### Statements with function/ procedure calls

When a statement involves a function/ procedure call, the complexity of the statement includes the complexity of the function/ procedure. Assume that you know that function/procedure `f` takes constant time, and that function/procedure `g` takes time proportional to (linear in) the value of its parameter `k`. Then the statements below have the time complexities indicated.

`f(k)` has `O(1)`
`g(k)` has `O(k)`

When a loop is involved, the same rule applies. For example:

```
for J in 1 .. N loop
  g(J);
end loop;
```

has complexity `(N2)`. The loop executes N times and each function/procedure call `g(N)` is complexity `O(N)`.


To simplify the running time estimation,    for a function f(n), we ignore the constants and lower order terms.
```
	Example: 10n3+4n2-4n+5  is O(n3).  

	7n-2

	7n-2 is O(n)
need c > 0 and n0 >= 1 such that 7n-2 <= c•n for n >= n0
this is true for c = 7 and n0 = 1

3n3 + 20n2 + 5

3n3 + 20n2 + 5 is O(n3)
need c > 0 and n0 >= 1 such that 3n3 + 20n2 + 5 <= c•n3 for n >= n0
this is true for c = 4 and n0 = 21

3 log n + 5

3 log n + 5 is O(log n)
need c > 0 and n0 >= 1 such that 3 log n + 5 <= c•log n for n >= n0
this is true for c = 8 and n0 = 2
```

Comparing O(N2) to O(N log N)

```
N		N log N		N2
2		    2		4
8		    24		64
32		    160		1024
128		    896		16,384
512		    4,608	262,144
2,048	    22,528	4,194,304
```


```
   -  O(n): If I double the input size the runtime doubles

   -  O(n2): If the input size doubles the runtime quadruples

   -  O(log n): If the input size doubles the runtime increases by one

   -  O(2n): If the input size increases by one, the runtime doubles
```

#### Big-O Complexity Chart
![Big-O Complexity Chart](http://bigocheatsheet.com/img/big-o-complexity.png)

Question

What is the average case Big O of linear search in an array with N items, if an item is present?
O(N)
O(N2) 
O(1)
O(logN)
O(NlogN)



# Algorithms
# Simple Sorting


##### Where it happens?
1.An internal sort requires that the collection of data fit entirely in the computer’s main memory.
2.We can use an external sort  when  the collection of data cannot fit in the computer’s main memory all at once but must reside in secondary storage such as on a disk.

The first three are the foundations for faster and more efficient algorithms.

### Bubble Sort

![Bubble Sort animation](https://raw.githubusercontent.com/reach2arunprakash/interview/master/bubble-sort.gif)

[Implementation](https://github.com/reach2arunprakash/interview/blob/master/src/main/java/com/zhokhov/interview/sorting/BubbleSort.java)

The bubble sort is notoriously slow, but it’s conceptually the simplest of the sorting algorithms.

##### Sorting process

1. Compare two items.
2. If the one on the left is bigger, swap them.
3. Move one position right.


##### Efficiency

For `10` data items, this is `45` comparisons (`9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1`).

In general, where `N` is the number of items in the array, there are `N-1` comparisons on the first pass, `N-2` on the second, and so on. The formula for the sum of such a series is
`(N–1) + (N–2) + (N–3) + ... + 1 = N*(N–1)/2 N*(N–1)/2 is 45 (10*9/2)` when `N` is `10`.

### Selection Sort

![Selection Sort animation](https://raw.githubusercontent.com/reach2arunprakash/interview/master/selection-sort.gif)

[Implementation](https://github.com/reach2arunprakash/interview/blob/master/src/main/java/com/zhokhov/interview/sorting/SelectionSort.java)

[Simple explanation](http://www.codenlearn.com/2011/07/simple-selection-sort.html)

The selection sort improves on the bubble sort by reducing the number of swaps necessary from `O(N2)` to `O(N)`. Unfortunately, the number of comparisons remains `O(N2)`. However, the selection sort can still offer a significant improvement for large records that must be physically moved around in memory, causing the swap time to be much more important than the comparison time.

##### Efficiency

The selection sort performs the same number of comparisons as the bubble sort: `N*(N-1)/2`. For `10` data items, this is `45` comparisons. However, `10` items require fewer than `10` swaps. With `100` items, `4,950` comparisons are required, but fewer than `100` swaps. For large values of `N`, the comparison times will dominate, so we would have to say that the selection sort runs in `O(N2)` time, just as the bubble sort did.

### Insertion Sort

![Insertion Sort animation](https://raw.githubusercontent.com/reach2arunprakash/interview/master/insertion-sort.gif)

[Implementation](https://github.com/reach2arunprakash/interview/blob/master/src/main/java/com/zhokhov/interview/sorting/InsertionSort.java)

[Simple explanation](http://www.codenlearn.com/2011/07/simple-insertion-sort.html)

In most cases the insertion sort is the best of the elementary sorts described in this chapter. It still executes in `O(N2)` time, but it’s about twice as fast as the bubble sort and somewhat faster than the selection sort in normal situations. It’s also not too complex, although it’s slightly more involved than the bubble and selection sorts. It’s often used as the final stage of more sophisticated sorts, such as quicksort.

##### Efficiency

How many comparisons and copies does this algorithm require? On the first pass, it compares a maximum of one item. On the second pass, it’s a maximum of two items, and so on, up to a maximum of N-1 comparisons on the last pass. This is `1 + 2 + 3 + ... + N-1 = N*(N-1)/2`

However, because on each pass an average of only half of the maximum number of items are actually compared before the insertion point is found, we can divide by 2, which gives `N*(N-1)/4`

The number of copies is approximately the same as the number of comparisons. However, a copy isn’t as time-consuming as a swap, so for random data this algorithm runs twice as fast as the bubble sort and faster than the selection sort.

In any case, like the other sort routines in this chapter, the insertion sort runs in `O(N2)` time for random data.

For data that is already sorted or almost sorted, the insertion sort does much better. When data is in order, the condition in the while loop is never true, so it becomes a simple statement in the outer loop, which executes `N-1` times. In this case the algorithm runs in `O(N)` time. If the data is almost sorted, insertion sort runs in almost `O(N)` time, which makes it a simple and efficient way to order a file that is only slightly out of order.

# Advanced Sorting

### Merge Sort

![Merge Sort animation](https://raw.githubusercontent.com/reach2arunprakash/interview/master/merge-sort.gif)

[Implementation](https://github.com/reach2arunprakash/interview/blob/master/src/main/java/com/zhokhov/interview/sorting/MergeSort.java)

[Simple explanation](http://www.codenlearn.com/2011/10/simple-merge-sort.html)

mergesort is a much more efficient sorting technique than those we saw in "Simple Sorting", at least in terms of speed. While the bubble, insertion, and selection sorts take `O(N2)` time, the mergesort is `O(N*logN)`.

For example, if `N` (the number of items to be sorted) is `10,000`, then `N2` is `100,000,000`, while `N*logN` is only `40,000`. If sorting this many items required `40` seconds with the mergesort, it would take almost `28` hours for the insertion sort.

The mergesort is also fairly easy to implement. It’s conceptually easier than quicksort and the Shell short.

The heart of the mergesort algorithm is the merging of two already-sorted arrays. Merging two sorted arrays `A` and `B` creates a third array, `C`, that contains all the elements of `A` and `B`, also arranged in sorted order.

Similar to quicksort the list of element which should be sorted is divided into two lists. These lists are sorted independently and then combined. During the combination of the lists the elements are inserted (or merged) on the correct place in the list.

You divide the half into two quarters, sort each of the quarters, and merge them to make a sorted half.

##### Sorting process

1. Assume the size of the left array is k, the size of the right array is m and the size of the total array is n (=k+m).
2. Create a helper array with the size n
3. Copy the elements of the left array into the left part of the helper array. This is position 0 until k-1.
4. Copy the elements of the right array into the right part of the helper array. This is position k until m-1.
5. Create an index variable i=0; and j=k+1
6. Loop over the left and the right part of the array and copy always the smallest value back into the original array. Once i=k all values have been copied back the original array. The values of the right array are already in place.

##### Efficiency

As we noted, the mergesort runs in `O(N*logN)` time. There are `24` copies necessary to sort `8` items. `Log28` is `3`, so `8*log28` equals `24`. This shows that, for the case of `8` items, the number of copies is proportional to `N*log2N`.

In the mergesort algorithm, the number of comparisons is always somewhat less than the number of copies.

####What you need to know:
- This is one of the most basic sorting algorithms.
- Know that it divides all the data into as small possible sets then compares them.

####Big O efficiency:
- Best Case Sort: Merge Sort: O(n)
- Average Case Sort: Merge Sort: O(n log n)
- Worst Case Sort: Merge Sort: O(n log n)

##### Comparison with Quicksort

Compared to quicksort the mergesort algorithm puts less effort in dividing the list but more into the merging of the solution.

Quicksort can sort "inline" of an existing collection, e.g. it does not have to create a copy of the collection while Standard mergesort does require a copy of the array although there are (complex) implementations of mergesort which allow to avoid this copying.

### Quick Sort

![Quick Sort animation](https://raw.githubusercontent.com/reach2arunprakash/interview/master/quick-sort.gif)

[Implementation](https://github.com/reach2arunprakash/interview/blob/master/src/main/java/com/zhokhov/interview/sorting/QuickSort.java)

[Simple explanation](http://me.dt.in.th/page/Quicksort/)
[Simple explanation 2](http://www.mycstutorials.com/articles/sorting/quicksort)

Quicksort is undoubtedly the most popular sorting algorithm, and for good reason: In the majority of situations, it’s the fastest, operating in `O(N*logN)` time. (This is only true for internal or in-memory sorting; for sorting data in disk files, other algorithms may be better.)

To understand quicksort, you should be familiar with the partitioning algorithm.

Quicksort algorithm operates by partitioning an array into two sub-arrays and then calling itself recursively to quicksort each of these subarrays.

##### Sorting process

[Preview](https://rawgit.com/reach2arunprakash/interview/master/quick-sort.html)

If the array contains only one element or zero elements then the array is sorted.

If the array contains more then one element then:

1. Select an element from the array. This element is called the "pivot element". For example select the element in the middle of the array.
2. All elements which are smaller then the pivot element are placed in one array and all elements which are larger are placed in another array.
3. Sort both arrays by recursively applying Quicksort to them.
4. Combine the arrays.

Quicksort can be implemented to sort "in-place". This means that the sorting takes place in the array and that no additional array need to be created.

##### Efficiency

Quicksort operates in `O(N*logN)` time. This is generally true of the divide-and-conquer algorithms, in which a recursive method divides a range of items into two groups and then calls itself to handle each group. In this situation the logarithm actually has a base of `2`: The running time is proportional to `N*log2N`.

####What you need to know:
- While it has the same Big O as (or worse in some cases) many other sorting algorithms it is often faster in practice than many other sorting algorithms, such as merge sort.
- Know that it halves the data set by the average continuously until all the information is sorted.

####Big O efficiency:
- Best Case Sort: Merge Sort: O(n)
- Average Case Sort: Merge Sort: O(n log n)
- Worst Case Sort: Merge Sort: O(n^2)

##### Standard Java Array sorting

Java offers a standard way of sorting Arrays with `Arrays.sort()`. This sort algorithm is a modified quicksort which show more frequently a complexity of `O(n log(n))`. See the Javadoc for details.


Since jdk7 many changes regarding sorts of options has occured, this was discussed many time, there are many information and articles on this subject, you can easily google it.
In short, here is the actual list of sorting implementations in the jdk: TimSort - for sorting objects by default, mergesort - also used for objects, the old version (enabled through a system property), Dual-Pivot Quick sort - for primitives, then sorting by counting is used for byte / character arrays and finally insertion sort is used for small arrays used in any case.


## Basic Types of Algorithms
###**Recursive Algorithms**
####Definition:
- An algorithm that calls itself in its definition.
  - **Recursive case** a conditional statement that is used to trigger the recursion.
  - **Base case** a conditional statement that is used to break the recursion.

####What you need to know:
- **Stack level too deep** and **stack overflow**.
  - If you've seen either of these from a recursive algorithm, you messed up.
  - It means that your base case was never triggered because it was faulty or the problem was so massive you ran out of RAM before reaching it.
  - Knowing whether or not you will reach a base case is integral to correctly using recursion.
  - Often used in Depth First Search


###**Iterative Algorithms**
####Definition:
- An algorithm that is called repeatedly but for a finite number of times, each time being a single iteration.
  - Often used to move incrementally through a data set.

####What you need to know:
- Generally you will see iteration as loops, for, while, and until statements.
- Think of iteration as moving one at a time through a set.
- Often used to move through an array.

####Recursion Vs. Iteration
- The differences between recursion and iteration can be confusing to distinguish since both can be used to implement the other. But know that,
  - Recursion is, usually, more expressive and easier to implement.
  - Iteration uses less memory.
- **Functional languages** tend to use recursion. (i.e. Haskell)
- **Imperative languages** tend to use iteration. (i.e. Ruby)
- Check out this [Stack Overflow post](http://stackoverflow.com/questions/19794739/what-is-the-difference-between-iteration-and-recursion) for more info.

####Pseudo Code of Moving Through an Array (this is why iteration is used for this)
```
Recursion                         | Iteration
----------------------------------|----------------------------------
recursive method (array, n)       | iterative method (array)
  if array[n] is not nil          |   for n from 0 to size of array
    print array[n]                |     print(array[n])
    recursive method(array, n+1)  |
  else                            |
    exit loop                     |
```
###**Greedy Algorithm**
####Definition:
- An algorithm that, while executing, selects only the information that meets a certain criteria.
- The general five components, taken from [Wikipedia](http://en.wikipedia.org/wiki/Greedy_algorithm#Specifics):
  - A candidate set, from which a solution is created.
  - A selection function, which chooses the best candidate to be added to the solution.
  - A feasibility function, that is used to determine if a candidate can be used to contribute to a solution.
  - An objective function, which assigns a value to a solution, or a partial solution.
  - A solution function, which will indicate when we have discovered a complete solution.

####What you need to know:
- Used to find the optimal solution for a given problem.
- Generally used on sets of data where only a small proportion of the information evaluated meets the desired result.
- Often a greedy algorithm can help reduce the Big O of an algorithm.

####Pseudo Code of a Greedy Algorithm to Find Largest Difference of any Two Numbers in an Array.
```
greedy algorithm (array)
  var largest difference = 0
  var new difference = find next difference (array[n], array[n+1])
  largest difference = new difference if new difference is > largest difference
  repeat above two steps until all differences have been found
  return largest difference
```

This algorithm never needed to compare all the differences to one another, saving it an entire iteration.

# Data Structures
## Data Structure Basics

###**Array**
####Definition:
- Stores data elements based on an sequential, most commonly 0 based, index.
- Based on [tuples](http://en.wikipedia.org/wiki/Tuple) from set theory.
- They are one of the oldest, most commonly used data structures.  

####What you need to know:
- Optimal for indexing; bad at searching, inserting, and deleting (except at the end).
- **Linear arrays**, or one dimensional arrays, are the most basic.
  - Are static in size, meaning that they are declared with a fixed size.
- **Dynamic arrays** are like one dimensional arrays, but have reserved space for additional elements.
  - If a dynamic array is full, it copies it's contents to a larger array.
- **Two dimensional arrays** have x and y indices like a grid or nested arrays.  
  
####Big O efficiency:
- Indexing:         Linear array: O(1),      Dynamic array: O(1)
- Search:           Linear array: O(n),      Dynamic array: O(n)
- Optimized Search: Linear array: O(log n), Dynamic array: O(log n)
- Insertion:        Linear array: n/a        Dynamic array: O(n)

###**Linked List**
####Definition: 
- Stores data with **nodes** that point to other nodes.
  - Nodes, at its most basic it has one datum and one reference (another node).
  - A linked list _chains_ nodes together by pointing one node's reference towards another node.  

####What you need to know:
- Designed to optimize insertion and deletion, slow at indexing and searching.
- **Doubly linked list** has nodes that reference the previous node.
- **Circularly linked list** is simple linked list whose **tail**, the last node, references the **head**, the first node.

####Big O efficiency:
- Indexing:         Linked Lists: O(n)
- Search:           Linked Lists: O(n)
- Optimized Search: Linked Lists: O(n)
- Insertion:        Linked Lists: O(1)  


### Stacks

A stack allows access to only one data item: the last item inserted. If you remove this item, you can access the next-to-last item inserted, and so on.

A stack is also a handy aid for algorithms applied to certain complex data structures. In "Binary Trees", we’ll see it used to help traverse the nodes of a tree.

Notice how the order of the data is reversed. Because the last item pushed is the first one popped.

commonly implemented with linked lists but can be made from arrays too.
  - Stacks are **last in, first out** (LIFO) data structures.
  - Made with a linked list by having the head be the only place for insertion and removal.

##### Efficiency

Items can be both pushed and popped from the stack implemented in the Stack class in constant `O(1)` time. That is, the time is not dependent on how many items are in the stack and is therefore very quick. No comparisons or moves are necessary.

### Queues

A queue is a data structure that is some- what like a stack, except that in a queue the first item inserted is the first to be removed (First-In-First-Out, `FIFO`), while in a stack, as we’ve seen, the last item inserted is the first to be removed (`LIFO`).

too can be implemented with a linked list or an array.
  - Queues are a **first in, first out** (FIFO) data structure.
  - Made with a doubly linked list that only removes from head and adds to tail.  
  
#### Deques

A deque is a double-ended queue. You can insert items at either end and delete them from either end. The methods might be called `insertLeft()` and `insertRight()`, and `removeLeft()` and `removeRight()`.

#### Priority Queues

A priority queue is a more specialized data structure than a stack or a queue. However, it’s a useful tool in a surprising number of situations. Like an ordinary queue, a priority queue has a front and a rear, and items are removed from the front. However, in a priority queue, items are ordered by key value so that the item with the lowest key (or in some implementations the highest key) is always at the front. Items are inserted in the proper position to maintain the order.

Difference between a heap and a priority queue

A priority queue can have any implementation, like a array that you search linearly when you pop. All it means is that when you pop you get the value with either the minimum or the maximum depending.

A classic heap as it is typically referred to is usually a min heap. An implementation that has good time complexity (O(log n) on push and pop) and no memory overhead.

In short, a priority queue can be implemented using many of the data structures that we've already studied (an array, a linked list, or a binary search tree). However, those data structures do not provide the most efficient operations. To make all of the operations very efficient, we'll use a new data structure called a heap.

##### Efficiency

In the priority-queue implementation we show here, insertion runs in `O(N)` time, while deletion takes `O(1)` time.

### Linked List

Arrays had certain disadvantages as data storage structures. In an unordered array, searching is slow, whereas in an ordered array, insertion is slow. In both kinds of arrays, deletion is slow. Also, the size of an array can’t be changed after it’s created.

We’ll look at a data storage structure that solves some of these problems: the linked list. Linked lists are probably the second most commonly used general-purpose storage structures after arrays.

#### Links

In a linked list, each data item is embedded in a link. A link is an object of a class called something like Link. Each Link object contains a reference (usually called next) to the next link in the list.

The LinkList class contains only one data item: a reference to the first link on the list. This reference is called first. It’s the only permanent information the list maintains about the location of any of the links. It finds the other links by following the chain of references from first, using each link’s next field.

#### Double-Ended Lists

A double-ended list is similar to an ordinary linked list, but it has one additional feature: a reference to the last link as well as to the first.

The reference to the last link permits you to insert a new link directly at the end of the list as well as at the beginning. Of course, you can insert a new link at the end of an ordinary single-ended list by iterating through the entire list until you reach the end, but this approach is inefficient.

Access to the end of the list as well as the beginning makes the double-ended list suitable for certain situations that a single-ended list can’t handle efficiently. One such situation is implementing a queue; we’ll see how this technique works in the next section.

##### Linked-List Efficiency

Insertion and deletion at the beginning of a linked list are very fast. They involve changing only one or two references, which takes `O(1)` time.

Finding, deleting, or inserting next to a specific item requires searching through, on the average, half the items in the list. This requires `O(N)` comparisons. An array is also `O(N)` for these operations, but the linked list is nevertheless faster because nothing needs to be moved when an item is inserted or deleted. The increased effi- ciency can be significant, especially if a copy takes much longer than a comparison.

Of course, another important advantage of linked lists over arrays is that a linked list uses exactly as much memory as it needs and can expand to fill all of available memory.

#### Sorted Lists

In the linked lists we’ve seen thus far, there was no requirement that data be stored in order. However, for certain applications it’s useful to maintain the data in sorted order within the list. A list with this characteristic is called a sorted list.

In a sorted list, the items are arranged in sorted order by key value. Deletion is often limited to the smallest (or the largest) item in the list, which is at the start of the list, although sometimes `find()` and `delete()` methods, which search through the list for specified links, are used as well.

##### Efficiency of Sorted Linked Lists

Insertion and deletion of arbitrary items in the sorted linked list require `O(N)` comparisons (`N/2` on the average) because the appropriate location must be found by stepping through the list. However, the minimum value can be found, or deleted, in `O(1)` time because it’s at the beginning of the list. If an application frequently accesses the minimum item, and fast insertion isn’t critical, then a sorted linked list is an effective choice. A priority queue might be implemented by a sorted linked list, for example.

#### Doubly Linked Lists

Let’s examine another variation on the linked list: the doubly linked list (not to be confused with the double-ended list). What’s the advantage of a doubly linked list? A potential problem with ordinary linked lists is that it’s difficult to traverse backward along the list. A statement like
current=current.next
steps conveniently to the next link, but there’s no corresponding way to go to the previous link. 

The doubly linked list provides this capability. It allows you to traverse backward as well as forward through the list. The secret is that each link has two references to other links instead of one. The first is to the next link, as in ordinary lists. The second is to the previous link.

#### Doubly Linked List as Basis for Deques

A doubly linked list can be used as the basis for a deque. In a deque you can insert and delete at either end, and the doubly linked list provides this capability.

### Iterator

Objects containing references to items in data structures, used to traverse these structures, are commonly called iterators (or sometimes, as in certain Java classes, enumerators). 

### Hash Tables

One important concept is how a range of key values is transformed into a range of array index values. In a hash table this is accomplished with a hash function. However, for certain kinds of keys, no hash function is necessary; the key values can be used directly as array indices.

Thus, we look for a way to squeeze a range of 0 to more than `7,000,000,000,000` into the range `0` to `100,000`. A simple approach is to use the **modulo operator** (`%`), which finds the remainder when one number is divided by another:

```
arrayIndex = hugeNumber % arraySize;
```

This is an example of a hash function. It hashes (converts) a number in a large range into a number in a smaller range. 

##### Hashing Efficiency

Insertion and searching in hash tables can approach `O(1)` time. If no collision occurs, only a call to the hash function and a single array reference are necessary to insert a new item or find an existing item. This is the minimum access time.


####What you need to know:
- Designed to optimize searching, insertion, and deletion.
- **Hash collisions** are when a hash function returns the same output for two distinct outputs.
  - All hash functions have this problem.
  - This is often accommodated for by having the hash tables be very large.
- Hashes are important for associative arrays and database indexing.

####Big O efficiency:
- Indexing:         Hash Tables: O(1)
- Search:           Hash Tables: O(1)
- Insertion:        Hash Tables: O(1)  

### Algorithms and Data Structures of JDK 7

http://www.yetanothercoder.ru/2013/06/algorithms-and-data-structures-of-jdk-7.html

###**Binary Tree**
####Definition: 
- Is a tree like data structure where every node has at most two children.
  - There is one left and right child node.

####What you need to know:
- Designed to optimize searching and sorting.
- A **degenerate tree** is an unbalanced tree, which if entirely one-sided is a essentially a linked list.
- They are comparably simple to implement than other data structures.
- Used to make **binary search trees**.
  - A binary tree that uses comparable keys to assign which direction a child is.
  - Left child has a key smaller than it's parent node.
  - Right child has a key greater than it's parent node.
  - There can be no duplicate node.
  - Because of the above it is more likely to be used as a data structure than a binary tree.

####Big O efficiency:
- Indexing:  Binary Search Tree: O(log n)
- Search:    Binary Search Tree: O(log n)
- Insertion: Binary Search Tree: O(log n) 


## Search Basics
###**Breadth First Search**
####Definition:
- An algorithm that searches a tree (or graph) by searching levels of the tree first, starting at the root.
  - It finds every node on the same level, most often moving left to right. 
  - While doing this it tracks the children nodes of the nodes on the current level.
  - When finished examining a level it moves to the left most node on the next level.
  - The bottom-right most node is evaluated last (the node that is deepest and is farthest right of it's level). 

####What you need to know:
- Optimal for searching a tree that is wider than it is deep.
- Uses a queue to store information about the tree while it traverses a tree.
  - Because it uses a queue it is more memory intensive than **depth first search**.
  - The queue uses more memory because it needs to stores pointers
  
####Big O efficiency:
- Search: Breadth First Search: O(|E| + |V|)
- E is number of edges
- V is number of vertices

###**Depth First Search**
####Definition:
- An algorithm that searches a tree (or graph) by searching depth of the tree first, starting at the root.
  - It traverses left down a tree until it cannot go further.
  - Once it reaches the end of a branch it traverses back up trying the right child of nodes on that branch, and if possible left from the right children.
  - When finished examining a branch it moves to the node right of the root then tries to go left on all it's children until it reaches the bottom.
  - The right most node is evaluated last (the node that is right of all it's ancestors). 
  
####What you need to know:
- Optimal for searching a tree that is deeper than it is wide.
- Uses a stack to push nodes onto.
  - Because a stack is LIFO it does not need to keep track of the nodes pointers and is therefore less memory intensive than breadth first search.
  - Once it cannot go further left it begins evaluating the stack.
  
####Big O efficiency:
- Search: Depth First Search: O(|E| + |V|)
- E is number of edges
- V is number of vertices


####Breadth First Search Vs. Depth First Search
- The simple answer to this question is that it depends on the size and shape of the tree.
  - For wide, shallow trees use Breadth First Search
  - For deep, narrow trees use Depth First Search

####Nuances:
  - Because BFS uses queues to store information about the nodes and its children, it could use more memory than is available on your computer.  (But you probably won't have to worry about this.)
  - If using a DFS on a tree that is very deep you might go unnecessarily deep in the search. See [xkcd](http://xkcd.com/761/) for more information.
  - Breadth First Search tends to be a looping algorithm.
  - Depth First Search tends to be a recursive algorithm.


# Top 10 Object Oriented Design Principles

1. **DRY (Don't repeat yourself)** — avoids duplication in code
2. **Encapsulate what changes** — hides implementation detail, helps in maintenance
3. **Open Closed design principle** — open for extension, closed for modification
4. **SRP (Single Responsibility Principle)** — one class should do one thing and do it well
5. **DIP (Dependency Inversion Principle)** — don't ask, let framework give to you
6. **Favor Composition over Inheritance** — code reuse without cost of inflexibility
7. **LSP (Liskov Substitution Principle)** — sub type must be substitutable for super type
8. **ISP (Interface Segregation Pricinciple)** — avoid monilithic interface, reduce pain on client side
9. **Programming for Interface** — helps in maintenance, improves flexibility
10. **Delegation principle** — don't do all things by yourself, delegate it

### TODO

1. triangular numbers
2. heap sort, binary search (BST)
3. OBJECT ORIENTED DESIGN. Major concepts, are the use of patterns necessary?
4. How does dynamic recompilation work in Resin (or any other Java servlet container)
5. write a O(log(n)) function


Technical Interview Questions
=============================

**General**
- Find the most frequent integer in an array
- Find pairs in an integer array whose sum is equal to 10 (bonus: do it in linear time)
- Given 2 integer arrays, determine of the 2nd array is a rotated version of the 1st array.
  
    ```Ex. Original Array A={1,2,3,5,6,7,8} Rotated Array B={5,6,7,8,1,2,3}```
- Write fibbonaci iteratively and recursively (bonus: use dynamic programming)
- Find the only element in an array that only occurs once.
- Find the common elements of 2 int arrays
- Implement binary search of a sorted array of integers
- Implement binary search in a rotated array (ex. {5,6,7,8,1,2,3})
- Use dynamic programming to find the first X prime numbers
- Write a function that prints out the binary form of an int
- Implement parseInt
- Implement squareroot function
- Implement an exponent function (bonus: now try in log(n) time)
- Write a multiply function that multiples 2 integers without using *
- Given n points, return the top k points that are closest to the origin
- We’re going to find “Word Twins”, which are pairs of English words, at least 4 letters long, where the first three letters of one are the last three letters of another.  For example, “strategy” and “Egypt”.
- Given a 3*3 matrix, and 1-8 numbers in random order, 1 place as space. 
        Write code to find the min exchange of numbers to make the matrix in order.
            
            5 4 1           1 2 3
            3   2   --->    8   4
            7 8 6           7 6 5
- There is k parenthesis, write code to calculate how many permutations could have. 
    For 2 parenthesis, there is 2 permutations: ()() and (()).
- **HARD**: Given a function rand5() that returns a random int between 0 and 5, implement rand7()
- **HARD**: Given a 2D array of 1s and 0s, count the number of "islands of 1s" (e.g. groups of connecting 1s)

**Strings**
- Find the first non-repeated character in a String
- Reverse a String iteratively and recursively
- Determine if 2 Strings are anagrams
- Check if String is a palindrome
- Check if a String is composed of all unique characters
- Determine if a String is an int or a double
- **HARD**: Find the longest palindrome in a String
- **HARD**: Print all permutations of a String
- **HARD**: Given a single-line text String and a maximum width value, write the function 'String justify(String text, int maxWidth)' that formats the input text using full-justification, i.e., extra spaces on each line are equally distributed between the words; the first word on each line is flushed left and the last word on each line is flushed right

**Trees**
- Implement a BST with insert and delete functions
- Print a tree using BFS and DFS
- Write a function that determines if a tree is a BST
- Find the smallest element in a BST
- Find the 2nd largest number in a BST
- Given a binary tree which is a sum tree (child nodes add to parent), write an algorithm to determine whether the tree is a valid sum tree
- Find the distance between 2 nodes in a BST and a normal binary tree
- Print the coordinates of every node in a binary tree, where root is 0,0
- Print a tree by levels
- Given a binary tree which is a sum tree, write an algorithm to determine whether the tree is a valid sum tree
- Given a tree, verify that it contains a subtree.
- Convert binary tree to doubly linked list
- **HARD**: Find the max distance between 2 nodes in a BST.
- **HARD**: Construct a BST given the pre-order and in-order traversal Strings

**Stacks, Queues, and Heaps**
- Implement a stack with push and pop functions
- Implement a queue with queue and dequeue functions
- Find the minimum element in a stack in O(1) time
- Write a function that sorts a stack (bonus: sort the stack in place without extra memory)
- Implement a binary min heap. Turn it into a binary max heap
- **HARD**: Implement a queue using 2 stacks

**Linked Lists**
- Implement a linked list (with insert and delete functions)
- Find the Nth element in a linked list
- Remove the Nth element of a linked list
- Check if a linked list has cycles
- Given a circular linked list, find the node at the beginning of the loop. 

    ```Ex. A-->B-->C --> D-->E -->C, C is the node that begins the loop```
- Check whether a link list is a palindrome
- Reverse a linked list iteratively and recursively
- Given a linked list, where each node has a link to a random node in the list, make a copy of the entire list
- Given a singly LL A->B->C->D->E->F... convert to B->A->D->C->F->E...

**Sorting**
- Implement bubble sort
- Implement selection sort
- Implement insertion sort
- Implement merge sort
- Implement quick sort

**BSTs, Heaps, Search Algorithms, Sort Algorithms, Intersection, median, hashmap, caching system, basic algorithms** 

- Basic bitwise operations
- How do you program a min heap using Nodes
- Find the max value in an array. The array is "semi-sorted". 

    Ex. { 1 3 4 7 9 10 12 13 12 6 3 }
- Write a code that accepts integers as arrays and outputs the multiplication result as an array.
- Write a code that takes the coordinates of multiple rectangles as input and returns as output the coordinates of the rectangle that is the intersection of all the rectangles.
- Typical low level CS questions about sorting algorithms and operational cost.
- Median finding algorithm - find the median of 'n' numbers and a little bit of binary search tree implementation
- Find the largest rectangle with all 0s in an matrix with only 0 and 1.
- Convert char string to integer. 
- Find occurrences of a number in sorted array (allow duplicates). 
- If integer array used to store big integers (one integer store one digit), implement arithmetic operations.
- How to build a heap?
- What is the optimized version of the knn algorithm?
- Write a recursive function to calculate pascal's pyramid numbers. 
- A question related to binary search, which is a kind of weak spot and I always avoid using it.
- Explain Singleton structure, how to create
- Given k sorted pivots, write procedure partition in quicksort
- Find the median of three numbers.
- Generate all balanced parentheses combinations of given length.
- The second question asked, how to find two missing integers in an unsorted array
- Given an array of characters in it, how would you reverse it?
- Write a program to comparing two array, one being very large
- To generate a fibonacci number sequence, and discuss its time and space complexity
- To merge two sorted integer arrays.
- Returning the n-th element of a linked list.
- How to randomly select a number with equal probability from an array with unknown size?
 Write an algorithm to find the 3rd highest number from an array of random integers
- Given a sorted array of n integers that has been rotated an unknown number of times, write code to find an element in the array. Sorted in increasing order

    Input: find 5 in (15, 16, 19, 20, 25, 1, 3, 4, 5, 6, 10, 14) Output 8
- Implement a simple regular expression matching function

**Uncategorized**
- Given a max-heap, how do I find the top k items?
- Find the border length created from a conglomeration of various 2D rectangles.
- Write a minPeak function for a stack (function that returns the minimum element in the stack).
- Find the nth fib number
- Design a function in your favorite programming language to convert a camelCase string to all lowercase.
- Implement a hashset
- Given a corpus of valid words, design a function that takes a word as input and outputs all valid anagrams of that word. 
- Given an input of a 3D matrix of ones and zeros, count the number of contiguous 1-filled regions (as separated by 0-filled regions), as well as the size of the largest one (I think; doesn't really change the problem much).
- You have two sets. How would you know that they converge.
- Given a bag of nuts and a bag of bolts, each having a different size within a bag but exactly one match in the other bag, give a fast algorithm to find all matches
- Preorder traversal without recursion
- Find the largest possible difference in an array of integers, such that the smaller integer occurs earlier in the array.
- How to find if n numbers in a list sum up to an integer k?
- Find largest palindrome in string
- Make an anagram solver that returns all valid dictionary words given a set of characters.
- Sort a string by the order it's characters appear in another string
- Given a value k and an array , design an efficient algorithm that should output the number of pairs that sum up to k.
- How do you find three numbers that sum to 0? (in a list). Now can you do it under O(n^3)?
- Given a Fibonacci number, tell us which index it occurs at.
- Describe an algorithm that would find n numbers in a list that sum to 0.
- Given an array of n unsorted ints, with the condition that each number is at most k positions away from its final sorted position, give an efficient sorting algorithm
- Give an efficient solution for subset sum.
- Given two (i,j) coordinates of a cell in two dimensional matrix. These coordinates are the lower left and upper right corner of a rectangle contained within the matrix. Sum all the elements in the matrix. Time and space?
- String has all unique characters
- Two strings to see if one is a permutation
- Remove dups from an unsorted linked list
- Find kth algorithm of singly linked list
- Delete a node in the middle of a singly linked list given access to that node
- Write code to partition a linked list around a value x such that all nodes less than x come before all nodes greater than or equal to x
- Single array to implement three stacks
- Stack with min element
- Towers of Hanoi
- Queue using two stacks
- Sort a stack in ascending order using at most one additional stack to hold items but you may not copy the elements into any other data structure
- Function to see if a tree is balanced
- Graph algorithm, route between two nodes
- Create bst from sorted array with minimal height
- Binary tree is a bst
- A child is running up a staircase with n steps, and cah hop either 1 step, 2 steps, or 3 steps at a time. Implement a method to count how many possible ways the child can run up the stairs
- Two sorted arrays, A has a large enough buffer at the end to hold B, merge B into A in sorted order
- Write a method to sort an array of strings so that all the anagrams are next to each other
- Find an element in a sorted array that has been rotated an unknown number of times
- Implement a method that returns true if the edit distance between two strings is less than 2 (1 or 0) or false otherwise
- Given two lists of chars, return the first with removed characters that appear in the second list.




### Sources

1. [Data Structures and Algorithms in Java, second edition by Robert Lafore](http://rineshpk.weebly.com/uploads/1/8/2/0/1820991/data_structures_and_algorithms_in_javatqw_darksiderg.pdf)
2. [10 Object Oriented Design Principles Java Programmer should know](http://javarevisited.blogspot.com/2012/03/10-object-oriented-design-principles.html)
3. [Design Patterns](http://www.oodesign.com/)
4. [Algorithms for Dummies (Part 1): Big-O Notation and Sorting](http://adrianmejia.com/blog/2014/02/13/algorithms-for-dummies-part-1-sorting/)
5. [Big O notation](http://web.mit.edu/16.070/www/lecture/big_o.pdf)
6. [A beginner's guide to Big O notation](https://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation/)
7. [Big O Notation. Using not-boring math to measure code's efficiency](https://www.interviewcake.com/article/big-o-notation-time-and-space-complexity)
8. [Understanding Algorithm complexity, Asymptotic and Big-O notation](http://www.codenlearn.com/2011/07/understanding-algorithm-complexity.html)
9. [Big-O Algorithm Complexity Cheat Sheet](http://bigocheatsheet.com)
10. [Algorithms in Java](http://www.vogella.com/tutorials/JavaAlgorithms/article.html)
11. [Mergesort in Java](http://www.vogella.com/tutorials/JavaAlgorithmsMergesort/article.html)
12. [Quicksort in Java](http://www.vogella.com/tutorials/JavaAlgorithmsQuicksort/article.html)

```
You are free to use this code anywhere, copy, modify and redistribute at your own risk.
Your are solely responsibly for any damage that may occur by using this code.

This code is not tested for all boundary conditions. Use it at your own risk.
```
