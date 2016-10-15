# Min Stack

[Leetcode](https://leetcode.com/problems/min-stack/)

題意：

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.


解題思路：


```java

class MinStack {
    
    Stack<Integer> stack = new Stack<>();
    Stack<Integer> mStack = new Stack<>();
    public void push(int x) {
        stack.push(x);
        if (mStack.isEmpty() || mStack.peek() >= x) {
            mStack.push(x);
        }
    }

    public void pop() {
        if (!stack.isEmpty()) {
            int val = stack.pop();
            if (!mStack.isEmpty() && val == mStack.peek()) {
                mStack.pop();
            }
        }
    }

    public int top() {
        if (!stack.isEmpty()) {
            return stack.peek();
        }
        return -1;
    }

    public int getMin() {
        if (!mStack.isEmpty()) {
            return mStack.peek();
        }
        return -1;
    }
}

```