#Max Tree

[原題連結](http://www.lintcode.com/en/problem/max-tree/)

題意：給定一個陣列，以此陣列建構出 Max Tree，Max Tree的條件如下：

* 根節點為陣列中的最大值
* 左子樹為根節點左半邊所構成的max tree
* 右子樹為根節點右半邊所構成的max tree

解題思路：以自下而上的方式，使用stack來輔助找出對於每個陣列元素的左右第一個比它大的值，將還不確定parent的節點存放於stack，若已確定parent的節點，則接在parent後，把parent 存回 stack中。

走完陣列所有元素後，把stack中所有片段組合起來，回傳head即可。


```java
public TreeNode maxTree(int[] A) {
    if (A == null || A.length == 0) {
        return null;
    }
    
    Stack<TreeNode> stack = new Stack<TreeNode>();
    stack.push(new TreeNode(A[0]));
    
    for (int i = 1; i < A.length; i++) {
        if (A[i] < stack.peek().val) {
            TreeNode node = new TreeNode(A[i]);
            stack.push(node);
        } else {
            // 因是max tree，所以stack內是降序的，所以一但當前的數比stack頂端的值還大，
            // 就要不斷的pop出來，由於越在stack底部的數值一定是在陣列的左邊，
            // 所以要不斷的pop直到當前值小於stack的peek。
            // 且每次pop，就要不斷把pop出來的值的右邊指向之前那個點
            
            TreeNode n1 = stack.pop();
            while (!stack.isEmpty() && stack.peek().val < A[i]) {
                
                TreeNode n2 = stack.pop();
                n2.right = n1;
                n1 = n2;
            }
            TreeNode node = new TreeNode(A[i]);
            node.left = n1;
            stack.push(node);
        }
    }
    // 如果stack只剩下一個節點，直接pop出來返回即可
    // 若還有大於一個節點的話，則要不斷的pop出來接右子點
    // 這段與else裡面那段while是一樣的
    TreeNode head = stack.pop();
    while (!stack.isEmpty()) {
        TreeNode temp = stack.pop();
        temp.right = head;
        head = temp;
    }
    
    return head;
}
```


```java
public TreeNode maxTree(int[] A) {
    
    Stack<TreeNode> stack = new Stack<TreeNode>();

    for ( int i = 0 ; i < A.length+1 ; i++ ) {
        int num;
        if ( i < A.length ) {
            num = A[i];
        } else {
            num = Integer.MAX_VALUE;
        }
        TreeNode now = new TreeNode(num);
        if ( stack.isEmpty() || stack.peek().val > now.val ) {
            stack.push(now);
        } else {
            while ( stack.peek().val < now.val ) {
                TreeNode target = stack.pop();
                if ( stack.isEmpty() || stack.peek().val > now.val ) {
                    now.left = target;
                    stack.push(now);
                } else {
                    stack.peek().right = target;
                }
            }
        }
    }
    return stack.peek().left;
}
```