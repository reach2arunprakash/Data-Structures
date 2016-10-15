#Subarray Sum

[原題連結](http://www.lintcode.com/en/problem/subarray-sum/)

題意：

給定一個整數矩陣，請找出一個子矩陣，使得其數字之和等於0.輸出答案時，請返回左上數字和右下數字的坐標。

給定矩陣
```
[
  [1 ,5 ,7],
  [3 ,7 ,-8],
  [4 ,-8 ,9],
]
```
返回 ```[(1,1), (2,2)]```

挑戰
O(n3) 時間複雜度。

解題思路：

網友 [Yu Zhang](http://www.cnblogs.com/yuzhangcmu/p/4174507.html)提供以下解法

使用Map 來記錄index, sum的值。當遇到兩個index的sum相同時，表示從index1+1到index2是一個解。

注意：添加一個index = -1作為虛擬節點。這樣我們才可以記錄index1 = 0的解。

空間複雜度：O(N)

```java
public ArrayList<Integer> subarraySum(int[] nums) {
        
    ArrayList<Integer> res = new ArrayList<Integer>();
    
    if (nums == null || nums.length == 0) {
        return res;
    }
    
    int len = nums.length;
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    int sum = 0;
    map.put(0, -1);
    
    for (int i = 0; i < len; i++) {
        sum += nums[i];
        if (map.containsKey(sum)) {
            res.add(map.get(sum) + 1);
            res.add(i);
            break;
        }
        
        map.put(sum, i);
    }
    
    return res;
}
```

---
###Reference
1. http://www.cnblogs.com/yuzhangcmu/p/4174507.html