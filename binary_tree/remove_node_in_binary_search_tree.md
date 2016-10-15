# Remove Node in Binary Search Tree

[原題網址](http://www.lintcode.com/en/problem/remove-node-in-binary-search-tree/)

> Given a root of Binary Search Tree with unique value for each node.  Remove the node with given value. If there is no such a node with given value in the binary search tree, do nothing. You should keep the tree still a binary search tree after removal.

> 刪除二元搜索樹中的一個節點

先找到要刪除的節點，若不是現在的節點，根據大小往左或往右繼續找下去。若找到就呼叫刪除的函數 `removeRoot(TreeNode root, int value)` 。
```java
public TreeNode removeNode(TreeNode root, int value) {
    // write your code here
    if ( root == null ) {
        return null;
    }
    
    if ( root.val == value ) {      // romove from root;
        root = removeRoot(root, value);
    } else if ( value > root.val ) {
        root.right = removeNode(root.right, value);
    } else {
        root.left = removeNode(root.left, value);
    }
    return root;
}
```

刪除頭的時候有以下三種情況：

1. **沒有左子樹也沒有右子樹**，換言之，在一棵樹上最底層的葉子。這種情況非常簡單，我們直接刪除不用考慮太多，回傳一顆空的樹 `null` 。
2. **只有一左邊子樹或右邊子樹**，這種情況也很容易解決。將節點一邊的子樹直接整串提上來回傳即可。
3. **同時有左子樹與右子樹**，這是一個必須仔細思考的情況。這時候有兩種解法，一種是將左邊子樹中最大的的節點取代現在的節點；或是將右邊子樹中最小的節點取代現在的節點。程式碼將實現前種方法。一種取巧的想法，如果我們將左邊數中的最大值 `TreeNode target` 取代現在的節點時，代表這個節點將被從左子樹中移除，我們遞迴地呼叫 `removeNode(TreeNode root, int value)` 來獲得正確的左子樹。

```java
public TreeNode removeRoot(TreeNode root, int value) {
    
    if ( root.left == null && root.right == null ) {
        return null;
    } else if ( root.left == null || root.right == null ) {
        if ( root.left == null ) {
            return root.right;
        } else {
            return root.left;
        }
    } else {
        TreeNode target = findMax(root.left);
        target.left = removeNode(root.left, target.val);
        target.right = root.right;
        return target;
    }
}

public TreeNode findMax(TreeNode root) {
    while ( root != null && root.right != null ) {
        root = root.right;
    }
    return root;
}
```

