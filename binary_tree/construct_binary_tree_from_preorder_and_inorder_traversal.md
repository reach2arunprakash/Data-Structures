# Construct Binary Tree from Preorder and Inorder Traversal

[原題網址](http://www.lintcode.com/en/problem/construct-binary-tree-from-preorder-and-inorder-traversal/)

> Given preorder and inorder traversal of a tree, construct the binary tree.

> 給你一顆二元樹的前序遍歷和後續遍歷，要把原本的樹重新構建出來。

這道題無法直接拿出筆畫圖想一個完美的規律。解題關鍵在於善加利用中序遍歷帶來的優勢。回想中序遍歷在任一節點上的定義：`左子樹 -> 節點 -> 右子樹`，可以發現


```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    if ( preorder == null || inorder == null || preorder.length != inorder.length ) {
        return null;
    }
    return helper(inorder, 0, inorder.length-1, preorder, 0, preorder.length-1);
}

public TreeNode helper(int[] inorder, int inStart, int inEnd, int[] preorder, int preStart, int preEnd) {
    if ( inStart > inEnd || preStart > preEnd ) {
        return null;
    }
    
    TreeNode root = new TreeNode(preorder[preStart]);
    int position = findPosition(inorder, preorder[preStart], inStart, inEnd);
    
    root.left = helper(inorder, inStart, position-1, preorder, preStart+1, preEnd-inEnd+position);
    root.right = helper(inorder, position+1, inEnd, preorder, preEnd-inEnd+position+1, preEnd);
    
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

