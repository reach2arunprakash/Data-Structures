#Count of Smaller Number before itself

[]()

題意：對於陣列的每個元素，要算在他前面有哪些元素比他小。

解題思路：可以用暴力法，即兩個循環來一一查詢，時間複雜度為$$O(N^{2})$$。
另一個較優的解可以使用計數型線段樹來查找，對於每個A[i]，去找線段樹中 min 到 A[i]-1範圍內有多少數字比A[i]小，由於題目要求是要查在A[i]之前的數，因此需要在查找完後，再將A[i]加入。程式如下：

```java
public class Solution {
   /**
     * @param A: An integer array
     * @return: Count the number of element before this element 'ai' is 
     *          smaller than it and return count number array
     */ 
     
    public class SegmentTreeNode {
        int start, end, count;
        SegmentTreeNode left, right;
        
        public SegmentTreeNode(int start, int end) {
            this.start = start;
            this.end = end;
            this.count = 0;
            this.left = null;
            this.right = null;
        }
    }
    
    
    
    private void modifyST(SegmentTreeNode root, int value) {
        
        if (root.start == value && root.end == value) {
            root.count++;
            return;
        }
        
        int mid = (root.start + root.end) / 2;

        // 除了檢查value在mid左邊還是右邊，還要檢查是否在區間內，
        // 即檢查是否大於start或是小於end，否則會出錯。
        if (value <= mid && value >= root.start) {
            modifyST(root.left, value);
        } 
        
        if (value > mid && value <= root.end) {
            modifyST(root.right, value);
        }
        
        root.count = root.left.count + root.right.count;
        
    }
    
    private int queryST(SegmentTreeNode root, int start, int end) {
        
        if (root.start == start && root.end == end) {
            return root.count;
        }
        
        int mid = (root.start + root.end) / 2;
        int leftCount = 0;
        int rightCount = 0;
        
        if (start <= mid) {
            if (end > mid) {
                leftCount = queryST(root.left, start, mid);
            } else {
                leftCount = queryST(root.left, start, end);
            }
        }
        
        if (end > mid) {
            if (start <= mid) {
                rightCount = queryST(root.right, mid + 1, end);
            } else {
                rightCount = queryST(root.right, start, end);
            }
        }
        
        return leftCount + rightCount;
    }
    
    private SegmentTreeNode buildST(int start, int end) {
        
        if (start > end) {
            return null;
        }
        
        SegmentTreeNode root = new SegmentTreeNode(start, end);
        
        if (!(start == end)) {
            int mid = (start + end) / 2;
        
            if (start <= mid) {
                root.left = buildST(start, mid);
            } 
            if (end > mid) {
                root.right = buildST(mid + 1, end);
            }
        }
        
        return root;
    }
    
    SegmentTreeNode root;
    public ArrayList<Integer> countOfSmallerNumberII(int[] A) {
        
        ArrayList<Integer> res = new ArrayList<Integer>();
        
        if (A == null || A.length == 0) {
            return res;
        }
        
        // 因題目指定0-10000，所以直接建一棵範圍為0-10000的樹即可
        root = buildST(0, 10000);
        
        // 不斷的去檢查當前線段樹0到 A[i] - 1這範圍有多少數比A[i]小，
        // 接著再把A[i]插入線段樹中，以供下一個值來搜尋。
        for (int i = 0; i < A.length; i++) {
            int answer = 0;
            //避免負數傳進query中。
            if (A[i] > 0) {
                answer = queryST(root, 0, A[i] - 1);
            }
            modifyST(root, A[i]);
            res.add(answer);
        }
        
        return res;
        
        
        
    }
}
```

>Time Complexity：$$O(KlogN)$$，其中K為陣列中的個數。