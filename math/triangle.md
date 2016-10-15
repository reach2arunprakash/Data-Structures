#Triangle

[]()

題意：找出由頂到底的最少成本路徑

解題思路：從底部往上算, 不斷透過以下方程式更新陣列，最後回傳triangle[0][0]即可。

>```triangle[i][j] = min(triangle[i+1][j], triangle[i+1][j+1]) + triangle[i][j]```

```java
public int minimumTotal(List<List<Integer>> triangle) {
    
    if (triangle == null || triangle.size() == 0) {
        return 0;
    }
    
    for (int i = triangle.size() - 2; i >= 0; i--) {
        List<Integer> cur = triangle.get(i);
        List<Integer> child = triangle.get(i+1);
        for (int j = 0; j < cur.size(); j++) {
            int min = Math.min(child.get(j), child.get(j+1));
            cur.set(j, min + cur.get(j));
        }
    }
    
    return triangle.get(0).get(0);
}
```