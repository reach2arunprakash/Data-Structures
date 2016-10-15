# Lowest common Ancestor

[Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

題意：

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: 「The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).」
```
        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5
         ```
For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.


解題思路：

updated 2016.1.7

```java
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        boolean change=true;
        while(change){       //when p and q on the two side of BST (or one is equal to root), exit the loop
            change=false;
            while(p.val<root.val&&q.val<root.val){
                root=root.left;
                change=true;
            }

            while(p.val>root.val&&q.val>root.val){
                root=root.right;
                change=true;
            }
        }


        return root;

    }
}
```


```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return root;
        TreeNode current = root;
        while (current != null && current != p && current != q){
            if (current.val <= p.val && current.val <= q.val) {
                current = current.right;
            } else if (current.val >= p.val && current.val >= q.val){
                current = current.left;
            } else {
                break;
            }
        }
        return current;
    }

```
當每個節點只有left與right兩個指針時，使用以下解法，
如果A和B都在root裡，返回A和B的最近公共祖先
如果只有a在root為根的子樹裡，返回a
如果只有b在root為根的子樹裡，返回b
如果都不在，返回null
```java
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode A, TreeNode B) {
        // write your code here
        if (root == null) {
            return null;
        }

        // 若在路徑上先遇到了其中一個，代表該點即是要找的節點，因再往下找一定找不到了。
        if (root == A || root == B) {
            return root;
        }
        
        TreeNode left = lowestCommonAncestor(root.left, A, B);
        TreeNode right = lowestCommonAncestor(root.right, A, B);

        // 表示 p 與 q皆在根節點的父親的其中一邊，回傳root即可
        if (left != null && right != null) {
            return root;
        }
        
        //否則pq可能在左邊或是右邊，返回其中之一不為空的結果即可
        return left != null? left:right;
    }
}
```

若是只有父節點的話，先把兩個節點到root的path列出來，接著一一比較，一直比較到不同的上一個則是最低共同父節點

```java
public class Solution {
    /**
     * @param root: The root of the binary search tree.
     * @param A and B: two nodes in a Binary.
     * @return: Return the least common ancestor(LCA) of the two nodes.
     */
    private ArrayList<TreeNode> getPath2Root(TreeNode node) {
        ArrayList<TreeNode> list = new ArrayList<TreeNode>();
        while (node != null) {
            list.add(node);
            node = node.parent;
        }
        return list;
    }
    public TreeNode lowestCommonAncestor(TreeNode node1, TreeNode node2) {
        ArrayList<TreeNode> list1 = getPath2Root(node1);
        ArrayList<TreeNode> list2 = getPath2Root(node2);

        int i, j;
        for (i = list1.size() - 1, j = list2.size() - 1; i >= 0 && j >= 0; i--, j--) {
            if (list1.get(i) != list2.get(j)) {
                return list1.get(i).parent;
            }
        }
        return list1.get(i+1);
    }


}

```
---
###Reference
1. http://wp.javayu.me/2014/02/lowest-common-ancestor-of-a-binary-tree/