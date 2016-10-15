#Triangle
[原題網址](http://www.lintcode.com/en/problem/triangle/)

```java
public int minimumTotal(ArrayList<ArrayList<Integer>> triangle) {

    if (triangle == null || triangle.get(0) == null | triangle.get(0).get(0) == null) {
        return 0;
    }
    
    int size = triangle.size();
    int[][] res = new int[size][size];
    
    for (int i = 0; i < size; i++) {
        res[size - 1][i] = triangle.get(size-1).get(i);
    }
    
    for (int i = size - 2; i >= 0; i--) {
        ArrayList<Integer> curList = triangle.get(i);
        for (int j = 0; j < curList.size(); j++) {
            res[i][j] = Math.min(res[i+1][j], res[i+1][j+1]) + curList.get(j);
        }
    }
    
    return res[0][0];
}
```
>Time Complexity：O(N)