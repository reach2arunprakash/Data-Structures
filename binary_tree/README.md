#Binary Tree

```java
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: An integer.
     */
    // singlePath:由root往下走到任何點的最大路徑，可以不包含root
    // maxPath: 整顆樹上目前任何點到任何點的最大路徑，至少包含一個點。
    // 使用一個resulttype class來不斷update這兩個值
    public class ResultType {
        int singlePath;
        int maxPath;
        public ResultType(int singlePath, int maxPath) {
            this.singlePath = singlePath;
            this.maxPath = maxPath;
        }
    }
    public int maxPathSum(TreeNode root) {
        // write your code here

        ResultType res = helper(root);
        return res.maxPath;
    }

    public ResultType helper(TreeNode root) {
        if (root == null) {
            return new ResultType(0, Integer.MIN_VALUE);
        }

        ResultType left = helper(root.left);
        ResultType right = helper(root.right);

        //檢查由此root往下走的最大path，看是往右走較大還是往左走較大
        //記得，可以不含root，因此拿結果與0來比較
        int singlePath = Math.max(left.singlePath, right.singlePath) + root.val;
        singlePath = Math.max(0, singlePath);

        //此地方包含了三個情況
        //最大sum在左子樹，最大sum在右子樹或是包含左右子樹且跨過root
        int maxPath = Math.max(left.maxPath, right.maxPath);
        maxPath = Math.max(maxPath, left.singlePath + right.singlePath + root.val);
        return new ResultType(singlePath, maxPath);
    }
}
```

