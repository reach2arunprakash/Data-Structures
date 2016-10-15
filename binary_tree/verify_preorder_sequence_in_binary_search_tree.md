# Verify Preorder Sequence in Binary Search Tree

[Leetcode](https://leetcode.com/problems/verify-preorder-sequence-in-binary-search-tree/)

題意：

Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree.

You may assume each number in the sequence is unique.

Follow up:
Could you do it using only constant space complexity?

解題思路：

updated on 2016.1.10

注意找切割點的寫法，其他就是不斷的二分去檢查。

```java
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        if (preorder == null || preorder.length < 2) {
            return true;
        }
        
        return helper(preorder, 0, preorder.length - 1, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }
    
    private boolean helper(int[] preorder, int start , int end, int min, int max) {
        if (start > end) {
            return true;
        }
        
        int rootValue = preorder[start];
        if (rootValue < min || rootValue > max) {
            return false;
        }
        
        int rightIndex = start;
        while (rightIndex <= end && preorder[rightIndex] <= rootValue) {
            rightIndex++;
        }
        
        return helper(preorder, start + 1, rightIndex - 1, min, rootValue) &&
                helper(preorder, rightIndex, end, rootValue, max);
    }
}
```

update on 2015.12.3

遞迴解法，效率較低但程式碼較簡潔

```java
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        if(preorder == null || preorder.length == 0) return true;
        return verify(preorder, 0, preorder.length - 1);
    }
    
    private boolean verify(int[] a, int start, int end) {
        if(start >= end) {
            return true;
        }
        int pivot = a[start];
        int bigger = -1;
        
        for(int i = start + 1; i <= end; i++) {
            if(bigger == -1 && a[i] > pivot) bigger = i;
            if(bigger != -1 && a[i] < pivot) return false;
        }
        
        if(bigger == -1) {
            return verify(a, start + 1, end);
        } else {
            return verify(a, start + 1, bigger - 1) && verify(a, bigger, end);
        }
    }
}
```

用遞迴的暴力法，找出root後，再判斷第一個比root大的值，就為切割點，把值切成兩半遞迴下去作，但可能corner case沒考慮到，只過了38個case，其程式碼如下，之後再改進：

```java
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        if (preorder == null || preorder.length < 2) {
            return true;
        }
        
        return helper(preorder, 0, preorder.length - 1);
    }
    
    public boolean helper(int[] preorder, int start, int end) {
        if (start > end) {
            return true;
        }
        
        int root = preorder[start];
        int i;
        for (i = start + 1; i <= end; i++) {
            if (preorder[i] > root) {
                break;
            }
        }
        
        boolean left = helper(preorder, start + 1, i - 1);
        boolean right = helper(preorder, i, end);
        
        return left && right;
    }
}
```

可以使用類似 water container使用stack的方式來作，首先使用stack，裡面的值只可能是遞減的，如果目前的p比stack頂端的值還大的話，便不斷的把頂端的值pop到一個list中，list存的便是inorder的順序即遞增，因有效的bst的inorder是遞增的，如果找到一個值比inorder的最後一個數還小的話，則表示這是不是有效的bst。

其程式碼如下：


```java
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        if (preorder == null || preorder.length < 2) {
            return true;
        }
        
        ArrayList<Integer> inorder = new ArrayList<Integer>();
        Stack<Integer> s = new Stack<Integer>();
        for (int i = 0 ; i < preorder.length; i++) {
            // inorder 內的數是已經處理完了，如果後面還找得到比inorder當前最後一個數還小的話，
            // 則不是有效的bst
            if (inorder.size() != 0 && inorder.get(inorder.size() - 1) > preorder[i]) {
                return false;
            }
            while (!s.isEmpty() && s.peek() < preorder[i]) {
                inorder.add(s.pop());
            }
            s.push(preorder[i]);
        }
        
        return true;
    }
}
```

其實我們都是比inorder中的最後一個數，所以我們只要用一個low 值來紀錄當前inorder的最後一個數值即可，可省下一個list，程式碼如下：

```java
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        if (preorder == null || preorder.length < 2) {
            return true;
        }
        
        int low = Integer.MIN_VALUE;
        Stack<Integer> s = new Stack<Integer>();
        for (int i = 0 ; i < preorder.length; i++) {
            // inorder 內的數是已經處理完了，如果後面還找得到比inorder當前最後一個數還小的話，
            // 則不是有效的bst
            if (low > preorder[i]) {
                return false;
            }
            while (!s.isEmpty() && s.peek() < preorder[i]) {
                low = s.pop();
            }
            s.push(preorder[i]);
        }
        
        return true;
    }
}
```

其實最後我們連stack都可以省略，因我們只需要stack頂端的元素，但是此時我們就需要更改到原本的preorder array了，我們使用另一個idx來指向當前stack頂端的元素，idx指向的指比當前元素還小，則low改成當前的preorder[idx]，且idx - 1，表示已將頂端元素pop掉，最後要插入新的值到stack時，只要把idx+1，然後把idx指向的值改為當前的preorder[i]即可。


```java
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        if (preorder == null || preorder.length < 2) {
            return true;
        }
        
        int low = Integer.MIN_VALUE;
        int idx = -1;
        for (int i = 0 ; i < preorder.length; i++) {
            // inorder 內的數是已經處理完了，如果後面還找得到比inorder當前最後一個數還小的話，
            // 則不是有效的bst
            if (low > preorder[i]) {
                return false;
            }
            while (idx >= 0 && preorder[idx] < preorder[i]) {
                low = preorder[idx];
                idx -= 1;
            }
            idx += 1;
            preorder[idx] = preorder[i];
        }
        
        return true;
    }
}
```

---
###Reference
1. https://www.youtube.com/watch?v=oVGen17RUf0