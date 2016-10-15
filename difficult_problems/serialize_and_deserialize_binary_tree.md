# Serialize and Deserialize Binary Tree

[Leetcode](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

題意：

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following tree
```
    1
   / \
  2   3
     / \
    4   5
    ```
as "[1,2,3,null,null,4,5]", just the same as how LeetCode OJ serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.
Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

解題思路：

Serialization:

對樹作bfs走訪，若root不為null 則插入root值，且不管左右子樹是否為空，全插入queue中，否則插入"#"。

Deserialization:

可以想像serialize的string是形容一棵full bt的字串，只要遇到#則把該子樹assign為null，否則建立一個新的樹節點，並塞到queue中。

程式碼如下：


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
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) {
            return "";
        }
        
        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(root);
        while (!q.isEmpty()) {
            TreeNode cur = q.poll();
            
            if (cur != null) {
                sb.append(cur.val);
                sb.append(" ");
                q.offer(cur.left);
                q.offer(cur.right);
            } else {
                sb.append("#");
                sb.append(" ");
            }
        }
        
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data == null || data.length() == 0) {
            return null;
        }
        
        data = data.trim();
        
        String delim = " ";
        String[] arr = data.split(delim);
        
        Queue<TreeNode> q = new LinkedList<>();
        
        TreeNode root = new TreeNode(Integer.parseInt(arr[0]));
        q.offer(root);
        
        int i = 1;
        
        while (!q.isEmpty() && i < arr.length) {
            int size = q.size();
            for (int j = 0; j < size; j++) {
                TreeNode cur = q.poll();
                
                //left child
                if (!arr[i].equals("#")) {
                    cur.left = new TreeNode(Integer.parseInt(arr[i]));
                    q.offer(cur.left);
                } else {
                    cur.left = null;
                }
                
                i++;
                // right child
                if (!arr[i].equals("#")) {
                    cur.right = new TreeNode(Integer.parseInt(arr[i]));
                    q.offer(cur.right);
                } else {
                    cur.right = null;
                }
                i++;
            }
        }
        
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

---
###Reference
1. http://buttercola.blogspot.com/2015/10/leetcode-serialize-and-deserialize.html