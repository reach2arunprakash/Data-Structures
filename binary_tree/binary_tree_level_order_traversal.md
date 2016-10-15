# Binary Tree Level Order Traversal

用BFS來解，標準模板

```java
public class Solution {
    public ArrayList<ArrayList<Integer>> levelOrder(TreeNode root) {
        // write your code here
        ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
        Queue<TreeNode> queue = new LinkedList<TreeNode>();

        if (root == null) {
            return res;
        }
        queue.offer(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            ArrayList<Integer> tempRes = new ArrayList<Integer>();
            //用size來判斷上一層節點有幾個，來決定要poll幾次
            for (int i = 0; i < size; i++) {
                TreeNode curNode = queue.poll();
                if (curNode.left != null) {
                    queue.offer(curNode.left);
                }
                if (curNode.right != null) {
                    queue.offer(curNode.right);
                }
                tempRes.add(curNode.val);
            }
            res.add(new ArrayList<Integer>(tempRes));
        }

        return res;
    }
}
```
