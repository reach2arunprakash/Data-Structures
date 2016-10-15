# Template

1. Traversal(DFS)
Build a traverse function and call it in main function
```java
// preorder example
public void traverse( resultType result, TreeNode node) {
    if ( node == null ) {     -> condition for leaves
        return;
    }
    result.add(node.val);    -> add the value
    traverse(result, node.left);
    traverse(result, node.right);
}
```

2. Divide and Conquer (DFS)
分別去跑，最後再把結果合起來

```java
public class Solution {
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        // null or leaf
        if (root == null) {
            return result;
        }

        // Divide
        ArrayList<Integer> left = preorderTraversal(root.left);
        ArrayList<Integer> right = preorderTraversal(root.right);

        // Conquer
        result.add(root.val);
        result.addAll(left);
        result.addAll(right);
        return result;
    }
}
```
