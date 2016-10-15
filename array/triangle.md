#Triangle

[原題連結](http://www.lintcode.com/en/problem/triangle/)

題意：Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle

[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]

The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

解題思路：由下往上作，每一層找出下一層相鄰且較小成本的點，把該點成本與本身的成本相加即可，程式碼如下：

```java
public int minimumTotal(ArrayList<ArrayList<Integer>> triangle) {
    
    if (triangle == null || triangle.size() == 0) {
        return 0;
    }
    
    for (int i = triangle.size() - 2; i >= 0; i--) {
        
        for (int j = 0; j < triangle.get(i).size(); j++) {
            int candidateOne = triangle.get(i+1).get(j);
            int candidateTwo = triangle.get(i+1).get(j+1);
            int minCost = Math.min(candidateOne, candidateTwo) + triangle.get(i).get(j);
            triangle.get(i).set(j, minCost);
        }
    }
    
    return triangle.get(0).get(0);
}
```