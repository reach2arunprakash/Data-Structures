# Two Sum III - Data structure design

[Leetcode](https://leetcode.com/problems/two-sum-iii-data-structure-design/)

題意：

Design and implement a TwoSum class. It should support the following operations: add and find.

```add``` - Add the number to an internal data structure.
```find``` - Find if there exists any pair of numbers which sum is equal to the value.

For example,
```
add(1); add(3); add(5);
find(4) -> true
find(7) -> false
```

解題思路：

主要運用到map.entry這個方法把所有key皆找出來，key value分別代表number 與count，程式碼如下：

```java
public class TwoSum {

    // Add the number to an internal data structure.
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
	public void add(int number) {
	    if (map.containsKey(number)) {
	        map.put(number, map.get(number) + 1);
	    } else {
	        map.put(number, 1);
	    }
	}

    // Find if there exists any pair of numbers which sum is equal to the value.
	public boolean find(int value) {
	    for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
	        int i = entry.getKey();
	        int j = value - i;
	        if (i == j && entry.getValue() > 1 || (i != j && map.containsKey(j))) {
	            return true;
	        }
	    }
	    return false;
	}
}
```