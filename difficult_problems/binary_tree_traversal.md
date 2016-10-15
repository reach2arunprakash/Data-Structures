# Binary Tree Traversal

**Pre Order Traverse**
```java
public List<Integer> preorderTraversal(TreeNode root) {
    ArrayList<Integer> result = new ArrayList<Integer>();
    Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
    TreeNode p = root;
    while(!stack.isEmpty() || p != null) {
        if(p != null) {
            stack.push(p);
            result.add(p.val);   // Add before going to children
            p = p.left;
        } else {
            TreeNode node = stack.pop();
            p = node.right;   
        }
    }
    return result;
}
```

**In Order Traverse**
```java
public List<Integer> inorderTraversal(TreeNode root) {
    ArrayList<Integer> result = new ArrayList<Integer>();
    Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
    TreeNode p = root;
    while(!stack.isEmpty() || p != null) {
        if(p != null) {
            stack.push(p);
            p = p.left;
        } else {
            TreeNode node = stack.pop();
            result.add(node.val); // Add after all left children
            p = node.right;   
        }
    }
    return result;
}
```

**Post Order Traverse**
```java
public List<Integer> postorderTraversal(TreeNode root) {
    LinkedList<Integer> result = new LinkedList<Integer>();
    Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
    TreeNode p = root;
    while(!stack.isEmpty() || p != null) {
        if(p != null) {
            stack.push(p);
            result.addFirst(p.val); // Reverse the process of preorder
            p = p.right;            // Reverse the process of preorder
        } else {
            TreeNode node = stack.pop();
            p = node.left;          // Reverse the process of preorder
        }
    }
    return result;
}
```

---
###Reference
1. https://leetcode.com/discuss/71943/preorder-inorder-and-postorder-iteratively-summarization