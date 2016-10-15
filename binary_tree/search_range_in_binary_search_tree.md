# Search Range in Binary Search Tree

[原題網址](http://www.lintcode.com/en/problem/search-range-in-binary-search-tree/)

Divide & Conquer

```java
public ArrayList<Integer> searchRange(TreeNode root, int k1, int k2) {
    // write your code here
    ArrayList<Integer> result = new ArrayList<Integer>();
    if ( root == null ) {
        return result;
    }
    
    if ( root.val >= k1 && root.val <= k2 ) {
        ArrayList<Integer> left = searchRange(root.left, k1, root.val-1);
        ArrayList<Integer> right = searchRange(root.right, root.val+1, k2 );
        
        result.addAll(left);
        result.add(root.val);
        result.addAll(right);
    } else if ( root.val > k1) {
        ArrayList<Integer> left = searchRange(root.left, k1, k2);
        
        result.addAll(left);
    } else {
        ArrayList<Integer> right = searchRange(root.right, k1, k2);
        
        result.addAll(right);
    }
    
    return result;
    
}
```