# Maximum Depth of Binary Tree

[原題網址](http://www.lintcode.com/en/problem/maximum-depth-of-binary-tree/)

> Given a binary tree, find its maximum depth.

> The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

> 題目要求一棵樹的最大深度。

解題關鍵在於，一個樹任一點的最大深度是由 `左子樹的深度` 與 `右子樹的深度` 看誰大再 +1 而獲得，所以可以輕鬆利用 Divide & Conquer 將問題分成 `左子樹` 跟 `右子樹` 在合併時取最大值 +1。程式如下：

```java
public int maxDepth(TreeNode root) {
	if (root == null) { // 邊界條件
		return 0;
	}
	// Divide 
	int left = maxDepth(root.left);
	int right = maxDepth(root.right);
	// Conquer
	return Math.max(left, right) + 1;
}
```
再次提醒，當使用 Divide & Conquer 時，一定要將邊界條件 (Boundary Condition) 寫在前面，這樣才不會出現程式跳不出、停不住而導致的 Runtime Error！

分析一下時間複雜度，由於每個點我們都會走過一次，複雜度取決於這棵樹上有多少節點 n 可得：

>時間複雜度 O(n)


updated on 2015.12.26

Iterative solution
```java
public int maxDepth(TreeNode root) {
    if (root == null)
        return 0;

    Deque<TreeNode> stack = new LinkedList<TreeNode>();

    stack.push(root);
    int count = 0;

    while (!stack.isEmpty()) {
        int size = stack.size();
        while (size-- > 0) {
            TreeNode cur = stack.pop();
            if (cur.left != null)
                stack.addLast(cur.left);
            if (cur.right != null)
                stack.addLast(cur.right);
        }
        count++;

    }
    return count;

}
```
