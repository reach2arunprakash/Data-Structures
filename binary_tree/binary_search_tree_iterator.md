#Binary Search Tree Iterator

[原題網址](http://www.lintcode.com/en/problem/binary-search-tree-iterator/)

題意：

Design an iterator over a binary search tree with the following rules:

Elements are visited in ascending order (i.e. an in-order traversal)
```next()``` and ```hasNext()``` queries run in O(1) time in average.


Example

For the following binary search tree, in-order traversal by using iterator is ```[1, 6, 10, 11, 12]```

```
   10
 /    \
1      11
 \       \
  6       12
  
```
Challenge
Extra memory usage O(h), h is the height of the tree.

解題思路：此題是要我們作中序遍歷的 iterator ，我們可以利用Stack來幫助我們處理這個問題。

主要要想到中序遍歷是怎麼走的，一開始一從是從最左邊的元素先印出，所以一開始初始化時，我們便從根節點一路向左不斷的把經過的節點全存進 stack 中，接著要用 next() 時，一開始先檢查 stack 是否為空，若不為空，則 stack 頂端那個元素便是當下要回傳的值，

接下來要塞新的節點時，會遇到以下兩個狀況：

1. 該節點的右子節點不為空：則從右子節點一路向左不停的塞 stack
2. 若該節點的右子節點為空，則回到父節點後，再問父節點的右子節點一路向左不停的塞 stack。


其原始碼如下：

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 * Example of iterate a tree:
 * Solution iterator = new Solution(root);
 * while (iterator.hasNext()) {
 *    TreeNode node = iterator.next();
 *    do something for node
 * } 
 */
public class Solution {
    //@param root: The root of binary tree.
    
    Stack<TreeNode> stack;
    
    public Solution(TreeNode root) {
        stack = new Stack<TreeNode>();
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
    }

    //@return: True if there has next node, or false
    public boolean hasNext() {
        return !stack.isEmpty();
    }
    
    //@return: return next node
    public TreeNode next() {
        
        if (!stack.isEmpty()) {
            TreeNode cur = stack.peek();
            TreeNode prev = stack.pop();
            if (prev.right != null) {
               prev = prev.right;
               while (prev != null) {
                   stack.push(prev);
                   prev = prev.left;
               }
               
            }
            return cur; 
        }
        return null;
    }
}

```

updated 2015.11.26

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

public class BSTIterator {
    private Stack<TreeNode> stack = new Stack<TreeNode>();
    public BSTIterator(TreeNode root) {
        //start traverse root;
        if (root != null) {
            pushLeft(root);
            
        }
    }
    //keep traverse left subtree
    //since left subtree also have smallest number
    private void pushLeft(TreeNode node) {
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.empty();
    }

    /** @return the next smallest number */
    public int next() {
        int result = 0;
        if (!stack.empty()) {
            //pop a value from the top of the stack
            //then keep traverse right subtree.
            TreeNode node = stack.pop();
            result = node.val;
            
            //root and every elements in the left subtree have already push into stack
            //start traverse right subtree
            pushLeft(node.right);
        }
        return result;
    }
}

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = new BSTIterator(root);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

