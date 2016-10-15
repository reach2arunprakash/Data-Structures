# Rectangle Area

[Leetcode]https://leetcode.com/problems/rectangle-area/

題意：

ind the total area covered by two rectilinear rectangles in a 2D plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

解題思路：


```java
public class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int sums = (C - A) * (D - B) + (G - E) * (H - F);
        
        if (D < F || H < B) {
            return sums;
        }
        
        if (C < E || G < A) {
            return sums;
        }
        
        int right = Math.min(C, G);
        int left = Math.max(A, E);
        int top = Math.min(H, D);
        int bottom = Math.max(F, B);
        
        return sums - (right - left) * (top - bottom);
    }
}
```

---
###Reference
1. http://www.programcreek.com/2014/06/leetcode-rectangle-area-java/