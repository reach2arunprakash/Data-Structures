# Construct Binary Tree from Inorder and Postorder Traversal

[原題網址](http://www.lintcode.com/en/problem/construct-binary-tree-from-inorder-and-postorder-traversal/)

> Given inorder and postorder traversal of a tree, construct the binary tree.

> 給你一顆二元樹的後序遍歷和後續遍歷，要把原本的樹重新構建出來。

想法跟[Construct Binary Tree from Preorder and Inorder Traversal](construct_binary_tree_from_preorder_and_inorder_traversal.md)一模一樣


```java
public TreeNode buildTree(int[] inorder, int[] postorder) {
    if ( inorder == null || postorder == null || inorder.length != postorder.length ) {
        return null;
    }
    return helper(inorder, 0, inorder.length-1, postorder, 0, postorder.length-1);
}

public TreeNode helper(int[] inorder, int inStart, int inEnd, int[] postorder, int postStart, int postEnd) {
    if ( inStart > inEnd || postStart > postEnd ) {
        return null;
    }
    
    TreeNode root = new TreeNode(postorder[postEnd]);
    int position = findPosition(inorder, postorder[postEnd], inStart, inEnd);
    
    root.left = helper(inorder, inStart, position-1, postorder, postStart, postStart+position-inStart-1);
    root.right = helper(inorder, position+1, inEnd, postorder, postEnd-inEnd+position, postEnd-1);
    
    return root;
}

public int findPosition(int[] array, int key, int start, int end) {
    for ( int i = start ; i <= end ; i++ ) {
        if ( array[i] == key ) {
            return i;
        }
    }
    return -1;
}
```

