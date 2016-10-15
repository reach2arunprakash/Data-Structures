# Insert Node in a Binary Search Tree

[原題網址](http://www.lintcode.com/en/problem/insert-node-in-a-binary-search-tree/)

> Given a binary search tree and a new tree node, insert the node into the tree. You should keep the tree still be a valid binary search tree.

> 題目要新加入一個數子到二元搜索樹 BST 

有關二元搜索樹 [Binary Search Tree]() 的特性，之前介紹過
，`父節點`要比`左子樹`大並且比`右子樹`小。因此在解這題時，必須要一直比較每個節點，如果比一個節點 `node_A` 大，往節點的右子樹 `node_A.right` 比較；相反地，若是比結點小，往節點的左子樹 `node_A.left` 比較。當比較後我們找到一個空的位置 `null` ，就是我們要加入的點。先來看看題目給的輸入參數和要回傳的資料：

```
@param root: The root of the binary search tree.
@param node: insert this node into the binary search tree
@return: The root of the new binary search tree.
```

考慮到重複一直做相同動作`比較->左或右`，用遞迴的方式去編寫這個程式是最直覺的，並且想法是帶有 Divide & Conquer 的觀念。
如果新加入的節點 `node` 是要加入`右子樹`， `root` 的右子樹就是會一個加入 `node` 後回傳的一棵樹，也就是再呼叫程式自己本身而得到的樹：

```java
public TreeNode insertNode(TreeNode root, TreeNode node) {
    if ( root == null ) { // 邊界條件
        return node;
    }
    
    if ( node.val > root.val ) {
        root.right = insertNode(root.right, node);
    } else {
        root.left = insertNode(root.left, node);
    }
    return root;
}
```

這題有個小挑戰，如果不准使用遞回方式，要如何處理呢？面對樹的處理，若不是要將所有樹節點遍歷一次，僅是要尋找樹中的特定位置，個人建議輕鬆面對，只要把樹的觀念回到簡單 Linked List 去思考，都能輕鬆解決！回想 Linked List 的模板，每次都要在 `while` 迴圈最後讓 `head = head.next` 。在面對樹關鍵就是，什麼時候要往左邊的叉路`左子樹`，什麼時候要往右邊叉路`右子樹`前進？再來就是我們要找到將要加入新數字 `node` 的父節點 `prev` ，而非位置本身 `cur` ，不然會連不起來，程序如下：

```java
public class Solution {

    public TreeNode insertNode(TreeNode root, TreeNode node) {
        
        if (root == null) {
            return node;
        }
        TreeNode current = root;
        TreeNode previous = null;
        while ( current != null ) {
            previous = current;
            if ( current.val < node.val) {
                current = current.right;
            } else {  // if ( current.val > node.val)
                current = current.left;
            }
        }
        if ( previous.val < node.val) {
            previous.right = node;
        } else {
            previous.left = node;
        }
        return root;
    }
}
```

> 時間複雜度 O(logn)