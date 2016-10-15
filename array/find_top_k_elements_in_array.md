# Find Top K Elements in Array

[]()

題意：


解題思路：

使用min heap。

```java
class Solution {
	public List<Integer> findTopKElement(int[] nums, int k) {
		if (nums == null || nums.length < k || k == 0) {
			return -1;
		}

		PriorityQueue<Integer> q = new PriorityQueue<Integer>();
		for (int i = 0; i < nums.length, i++) {
			if (i > k && nums[i] > q.peek()) {
				q.poll();
			}
			q.offer(nums[i]);
		}

		List<Integer> res = new ArrayList<>();
		for (int i = 0; i < k; i++) {
			res.add(q.poll());
		}

		return res;
	}
}
```