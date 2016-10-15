# Unique Binary Search Trees II

[Leetcode](http://www.lintcode.com/en/problem/unique-binary-search-trees-ii/)

題意：

Given n, generate all structurally unique BST's (binary search trees) that store values 1...n.


Example

Given n = 3, your program should return all 5 unique BST's shown below.
```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

解題思路：

>引用 [Code Ganker](http://codeganker.blogspot.com/2014/04/unique-binary-search-trees-ii-leetcode.html) 的講解：

這道題是求解所有可行的二叉查找樹，從Unique Binary Search Trees中我們已經知道，可行的二叉查找樹的數量是相應的卡特蘭數，不是一個多項式時間的數量級，所以我們要求解所有的樹，自然是不能多項式時間內完成的了。

算法上還是用求解NP問題的方法來求解，也就是N-Queens中
介紹的在循環中調用遞歸函數求解子問題。思路是每次一次選取一個結點為根，然後遞歸求解左右子樹的所有結果，最後根據左右子樹的返回的所有子樹，依次選取
然後接上（每個左邊的子樹跟所有右邊的子樹匹配，而每個右邊的子樹也要跟所有的左邊子樹匹配，總共有左右子樹數量的乘積種情況），構造好之後作為當前樹的
結果返回。

這道題的解題依據依然是：
當數組為 1，2，3，4，.. i，.. n時，基於以下原則的BST建樹具有唯一性：
以i為根節點的樹，其左子樹由**[1, i-1]**構成， 其右子樹由**[i+1, n]**構成。 
代碼如下：

> 利用 left 與 right來限制當前子樹可用的所有值，主要就是 1 到 n 各當 root後，去枚舉左右子樹，假如 i = 5，則去枚舉1-4的所有可能左子樹，與6-10 的所有可能右子樹。

程式碼如下：

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @paramn n: An integer
     * @return: A list of root
     */
    public List<TreeNode> generateTrees(int n) {
        
        return helper(1, n);
    }
    
    public List<TreeNode> helper(int left, int right) {
        List<TreeNode> res = new ArrayList<TreeNode>();
        if (left > right) {
            res.add(null);
            return res;
        }
        
        for (int i = left; i <= right; i++) {
            // 以 i 為root，去枚舉所有的左右子樹，所有左子樹可能的組合全放入left，右子樹則放到right
            List<TreeNode> leftTrees = helper(left, i - 1);
            List<TreeNode> rightTrees = helper(i + 1, right);
            
            // 接著枚舉所有可能的左右子樹組合與 root接上後加到 res內
            for (int j = 0; j < leftTrees.size(); j++) {
                for (int k = 0; k < rightTrees.size(); k++) {
                    TreeNode root = new TreeNode(i);
                    root.left = leftTrees.get(j);
                    root.right = rightTrees.get(k);
                    res.add(root);
                }
            }
        }
        
        return res;
    }
}

```

updated 2015.11.22

```java
public class Solution {
    /**
     * @paramn n: An integer
     * @return: A list of root
     */
    public List<TreeNode> generateTrees(int n) {
        if (n == 0) {
            return new ArrayList<TreeNode>();
        }
        return helper(1,n);
    
    }
    
    public List<TreeNode> helper(int s, int e){
        List<TreeNode> res = new ArrayList<TreeNode>(); 
        if(s > e){
            res.add(null);
            return res;
        }
        for(int i=s; i<=e; i++){
    
            List<TreeNode> L = helper(s,i-1);
            List<TreeNode> R = helper(i+1,e);
            for(TreeNode l : L){
    
                for(TreeNode r : R){
                    TreeNode node = new TreeNode(i);
                    node.left = l;
                    node.right = r;
                    res.add(node);
                 }
            }
        }
        return res;
    }
}
```


---
###Reference
1. http://codeganker.blogspot.com/2014/04/unique-binary-search-trees-ii-leetcode.html
2. http://www.cnblogs.com/springfor/p/3884029.html
3. https://leetcode.com/discuss/59567/95%25-fast-concise-java-recursive-solution-with-explanation