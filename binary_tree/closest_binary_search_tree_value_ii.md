# Closest Binary Search Tree Value II

[]()

題意：

Given a non-empty binary search tree and a target value, find k values in the BST that are closest to the target.

Note:

Given target value is a floating point.
- You may assume k is always valid, that is: k ≦ total nodes.
- You are guaranteed to have only one unique set of k values in the BST that are closest to the target.

Follow up:

Assume that the BST is balanced, could you solve it in less than O(n) runtime (where n = total nodes)?

Hint: 

1. Consider implement these two helper functions:
    - getPredecessor(N), which returns the next smaller node to N.
    - getSuccessor(N), which returns the next larger node to N.
2. Try to assume that each node has a parent pointer, it makes the problem much easier.
3. Without parent pointer we just need to keep track of the path from the root to the current node using a stack.
4. You would need two stacks to track the path in finding predecessor and successor node separately.


解題思路：


使用priority queue，因queue中頂端的值是queue中所有元素最小的值，表示與target差距最大，所以只要不斷的與queue頂端的元素作比較即可，若數目已達k，且頂端元素與target的差距比新的元素與target的差距還大的話，則把頂端元素pop掉並插入新的元素，程式碼如下：


```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    private PriorityQueue<Integer> q;
    private int count = 0;
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        q = new PriorityQueue<Integer>();
        List<Integer> res = new ArrayList<>();
        if (root == null || k == 0) {
            return res;
        }
        
        inorderTraverse(root, target , k);
        for (Integer val : q) {
            res.add(val);
        }
        
        return res;
        
        
    }
    
    private void inorderTraverse(TreeNode root, double target, int k) {
        if (root == null) {
            return;
        }
        inorderTraverse(root.left, target, k);
        
        if (count < k) {
            q.offer(root.val);
        } else {
            if (Math.abs((double) root.val - target) < Math.abs((double)q.peek() - target)) {
                q.poll();
                q.offer(root.val);
            }
        }
        count++;
        inorderTraverse(root.right, target , k);
    }
}
```

另外可維護兩個predecessors與sucessors的stack，接著再從兩個輸出最小的k個數，其程式碼如下：


```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    Stack<Integer> predecessors;
    Stack<Integer> successors;
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        List<Integer> res = new ArrayList<>();
        predecessors = new Stack<>();
        successors = new Stack<>();
        if (root == null || k == 0) {
            return res;
        }
        getPredecessors(root, target);
        getSuccessors(root, target);
        
        for (int i = 0; i < k; i++) {
            if (predecessors.isEmpty()) {
                res.add(successors.pop());
            } else if (successors.isEmpty()) {
                res.add(predecessors.pop());
            } else if (Math.abs((double)predecessors.peek() - target) < Math.abs((double)successors.peek() - target)) {
                res.add(predecessors.pop());
            } else {
                res.add(successors.pop());
            }
        }
        
        return res;
    }
    
    
    private void getPredecessors(TreeNode root, double target) {
        if (root == null) {
            return;
        }
        
        getPredecessors(root.left, target);
        if (root.val > target) {
            return;
        }
        predecessors.push(root.val);
        
        getPredecessors(root.right, target);
    }
    
    private void getSuccessors(TreeNode root, double target) {
        if (root == null) {
            return;
        }
        
        getSuccessors(root.right, target);
        if (root.val <= target) {
            return;
        }
        successors.push(root.val);
        getSuccessors(root.left, target);
    }
}
```





---
###Reference
1. http://buttercola.blogspot.com/search?q=Closest+Binary+Search+Tree+Value