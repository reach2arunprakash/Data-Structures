#Binary Tree Path

[原題網址](https://leetcode.com/problems/binary-tree-paths/)

**題意：**

給定一個二元樹，印出所有根到葉節點的路徑。

**解題思路：**

使用深度優先來幫助解決這道問題，程式碼如下：



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
    
    List<String> res = new ArrayList<String>();
    public List<String> binaryTreePaths(TreeNode root) {
        
        if (root == null) {
            return res;
        }
        
        helper(root, new ArrayList<Integer>());
        
        return res;
    }
    
    private void helper(TreeNode root, List<Integer> tempRes) {
        if (root == null) {
            return;
        }
        if (root.right == null && root.left == null) {
            tempRes.add(root.val);
            String candidate = pathGenerator(tempRes);
            if (!res.contains(candidate)) {
                res.add(candidate);
            }
            res.remove(res.size() - 1);
            return;
        }
        
        tempRes.add(root.val);
        helper(root.left, tempRes);
        helper(root.right, tempRes);
        tempRes.remove(tempRes.size() - 1);
    }
    
    private String pathGenerator(List<Integer> list) {
        StringBuilder str = new StringBuilder();
        for (int i = 0; i < list.size(); i++) {
            if (i == list.size() - 1) {
                str.append(list.get(i));
            } else {
                str.append(list.get(i) + "->");
            }
        }
        
        return str.toString();
    }
}
```

updated on 2015.12.3

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
    List<String> res;
    public List<String> binaryTreePaths(TreeNode root) {
        res = new ArrayList<String>();
        if (root == null) {
            return res;
        }
        
        helper(root, new ArrayList<Integer>());
        return res;
    }
    
    private void helper(TreeNode root, List<Integer> nodes) {
        if (root == null) {
            return;
        }
        
        nodes.add(root.val);
        if (root.left == null && root.right == null) {
            res.add(genString(nodes));
        }
        if (root.left != null) {
            helper(root.left, nodes);
        }
        if (root.right != null) {
            helper(root.right, nodes);
        }
        
        nodes.remove(nodes.size() - 1);
        
    }
    
    private String genString(List<Integer> nodes) {
        StringBuilder sb = new StringBuilder();
        
        for (int i = 0; i < nodes.size(); i++) {
            if (i != 0) {
                sb.append("->");
            }
            sb.append(String.valueOf(nodes.get(i)));
        }
        return sb.toString();
    }
}
```