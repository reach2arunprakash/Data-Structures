#Expression Tree Build

[原題連結](http://www.lintcode.com/en/problem/expression-tree-build/)

題意：

```java
/**
 * Definition of ExpressionTreeNode:
 * public class ExpressionTreeNode {
 *     public String symbol;
 *     public ExpressionTreeNode left, rihttp://www.lintcode.com/en/problem/expression-tree-build/#ght;
 *     public ExpressionTreeNode(String symbol) {
 *         this.symbol = symbol;
 *         this.left = this.right = null;
 *     }
 * }
 */

public class Solution {
    
    class TreeNode {
        public int val;
        public String s;
        public ExpressionTreeNode root;
        
        public TreeNode(int val, String ss) {
            this.val = val;
            this.root = new ExpressionTreeNode(ss);
        }
    }
    /**
     * @param expression: A string array
     * @return: The root of expression tree
     */
    public ExpressionTreeNode build(String[] expression) {
        
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode root = null;
        int val = 0;
        Integer base = 0;

        for (int i = 0; i <= expression.length; i++) {
            
            if (i != expression.length) {
                
                //只要遇到開頭為(，則直接把base加10，代表後面的所有值的優先權都會額外加10
                if (expression[i].equals("(")) {
                    base += 10;
                    continue;
                }
                // 因已到了)的結尾，因此後面的數無需再加上額外10的優先權，把base扣10，繼續後面的運算。
                if (expression[i].equals(")")) {
                    base -= 10;
                    continue;
                }
                
                //得出當前值的優先權
                val = get(expression[i], base);
            }
            
            //抓到當前值的優先權後，便建立一個新節點right。
            TreeNode right = i == expression.length ?
                new TreeNode(Integer.MIN_VALUE, "") :
                new TreeNode(val, expression[i]);
                
            // 重點，對於每個點找出左右第一個比它大的，
            // 下面這段程式碼要來建 min tree
            while (!stack.isEmpty()) {
                
                // 與stack的頂值來比較，如果頁值比較大，
                // 代表找到左邊第一個他大的數，作進一步處理
                if (right.val <= stack.peek().val) {
                    
                    TreeNode nodeNow = stack.pop();
                    
                    // 如果stack已空了，由於是建min tree，且right的值也較小，
                    // 且stack內的值一定在right node的左邊，所以直接把right那個節點的left指到現在這個節點nodenow即可。
                    if (stack.isEmpty()) {
                        right.root.left = nodeNow.root;
                    } else {
                        
                        //如果stack還有節點的話，
                        
                        TreeNode left = stack.peek();
                        if (left.val < right.val) {
                            right.root.left = nodeNow.root;
                        } else {
                            left.root.right = nodeNow.root;
                        }
                    }
                } else {
                    break;
                }
               
            }
            
            stack.push(right);
        }
        
        return stack.peek().root.left;
    }
    
    // 用來指定優先權的，值越小優先權越高，operand 權限最小，設成無限大
    // + 與 - 其次，因此base + 1
    int get(String a, Integer base) {
        if (a.equals("+") || a.equals("-")) {
            return 1 + base;
        }
        
        if (a.equals("*") || a.equals("/")) {
            return 2 + base;
        }
        
        return Integer.MAX_VALUE;
    }
}

```
