#Max Points in a Line

[原題網址](http://www.lintcode.com/en/problem/max-points-on-a-line/)

題意：找出同一條線上的最多點數，即找出最多相同斜率的點。

解決方法：固定一點不斷去與其他點算斜率，並使用一hash table來幫助我們記憶，另外要注意相同座標的點，使用duplicate變數來計算，程式碼如下，(有最後兩個case無法過，回頭再修正)：

```java
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */
public class Solution {
    public int maxPoints(Point[] points) {
        
        if (points == null) {
            return 0;
        }
        
        if (points.length <= 2) {
            return points.length;
        }
        
        int len = points.length;
        HashMap<Double, Integer> map = new HashMap<Double, Integer>();
        int max = Integer.MIN_VALUE;
        
        for (int i = 0; i < len; i++) {
            
            int duplicate = 1;
            int tempMax = 0;
            double slope = 0.0;
            map.clear();
            Point A = points[i];
            Point B;
            
            for (int j = i + 1; j < len; j++) {
                B = points[j];
                if (i == j) {
                    continue;
                }
                
                // look 0.0+(double)(points[j].y-points[i].y)/(double)(points[j].x-points[i].x)
                // because (double)0/-1 is -0.0, so we should use 0.0+-0.0=0.0 to solve 0.0 !=-0.0
                // problem
                if (A.x != B.x) {
                    slope = 0.0 + (double)(A.y - B.y) / (double)(A.x - B.x);
                } else if ((A.x == B.x) && (A.y == B.y)) {
                    duplicate++;
                    continue;
                } else if (A.x == B.x) {
                    slope = Integer.MAX_VALUE;
                }
                
                if (map.containsKey(slope)) {
                    map.put(slope, map.get(slope) + 1);
                } else {
                    map.put(slope, 1);
                }
                
                if (map.get(slope) > tempMax) {
                    tempMax = map.get(slope);
                }
            }
            max = (max > (tempMax + duplicate)) ? max : (tempMax + duplicate);
        }
        
        return max;
    }
}
```
>Time Complexity ：$$O(N^{2})$$

---
###Reference

1. http://www.jiuzhang.com/solutions/max-points-on-a-line/