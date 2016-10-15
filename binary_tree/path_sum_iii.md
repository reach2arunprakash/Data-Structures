# Path Sum III



題意：

解題思路：

```java

public class Solution{
	public void findSum(TreeNode node, int sum, int[] path, int level) {
		if (node == null) {
			return;
		}

		path[level] = node.data;

		int t = 0;
		for (int i = level; i >= 0; i--) {
			t += path[i];
			if (t == sum) {
				print(path, i, level);
			}
		}

		findSum(node.left, sum, path, level + 1);
		findSum(node.right, sum, path, level + 1);

		path[level] = Integer.MIN_VALUE;
	}

	public void findSum(TreeNode node, int sum) {
		int depth = depth(node);
		int[] path = new int[depth];
		findSum(node, sum, path, 0);
	}

	public static void print(int[] path, int start, int end) {
		for (int i = start, i <= end; i++) {
			System.outprintln(path[i] + " ");
		}
		System.out.println();
	}

	public int depth(TreeNode node) {
		if (node == null) {
			return 0;
		} else {
			return 1 + Math.max(depth(node.left), depth(node.right));
		}
	}
}
```
