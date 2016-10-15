# Unique Binary Search Tree

題意：

Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

For example,
Given n = 3, there are a total of 5 unique BST's.
```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

解題思路：

一開始只知道可用 Catalan Number 來算，但不知怎列式子，多舉幾個例子即可推出算式。

>網友 [Code Ganker](http://blog.csdn.net/linhuanmars/article/details/21145563) 提供了以下解說：

「這道題要求可行的二叉查找樹的數量，其實二叉查找樹可以任意取根，只要滿足中序遍歷有序的要求就可以。從處理子問題的角度來看，選取一個結點為根，就把結點
切成左右子樹，以這個結點為根的可行二叉樹數量就是左右子樹可行二叉樹數量的乘積，所以總的數量是將以所有結點為根的可行結果累加起來。寫成表達式如下：

![](http://2.bp.blogspot.com/-aqaV4LBzCnI/UwuKL8OGwlI/AAAAAAAAALM/wJzuSqdm9r8/s1600/catalan.png)

熟悉卡特蘭數的朋友可能已經發現了，這正是卡特蘭數的一種定義方式，是一個典型的動態規劃的定義方式（根據其實條件和遞推式求解結果）。所以思路也很明確了，維護量res[i]表示含有i個結點的二叉查找樹的數量。根據上述遞推式依次求出1到n的的結果即可。

時間上每次求解i個結點的二叉查找樹數量的需要一個i步的循環，總體要求n次，所以總時間複雜度是O(1+2+...+n)=O(n^2)。空間上需要一個數組來維護，並且需要前i個的所有信息，所以是O(n)。"

>另外網友 [水中的魚]() 提供了更精闢的解說，如下：

「 這題想了好久才想清楚。其實如果把上例的順序改一下，就可以看出規律了。 

      1         3     3      2      1
       \       /     /      / \      \
        3     2     1      1   3      2
       /     /       \                 \
      2     1         2                 3
 

   比如，以1為根的樹有幾個，完全取決於有二個元素的子樹有幾種。同理，2為根的子樹取決於一個元素的子樹有幾個。以3為根的情況，則與1相同。

    定義Count[i] 為以[0,i]能產生的Unique Binary Tree的數目，

    如果數組為空，毫無疑問，只有一種BST，即空樹，
    Count[0] =1

    如果數組僅有一個元素{1}，只有一種BST，單個節點
    Count[1] = 1

    如果數組有兩個元素{1,2}， 那麼有如下兩種可能
    1                       2
     \                    /
       2                1
    Count[2] = Count[0] * Count[1]   (1為根的情況)
                  + Count[1] * Count[0]  (2為根的情況。

    再看一遍三個元素的數組，可以發現BST的取值方式如下：
    Count[3] = Count[0]*Count[2]  (1為根的情況)
                  + Count[1]*Count[1]  (2為根的情況)
                  + Count[2]*Count[0]  (3為根的情況)

    所以，由此觀察，可以得出Count的遞推公式為
    Count[i] = Σ Count[0...k] * [ k+1....i]     0<=k<i-1
    問題至此劃歸為一維動態規劃。

   [Note]
    這是很有意思的一個題。剛拿到這題的時候，完全不知道從那下手，因為對於BST是否Unique，很難判斷。最後引入了一個條件以後，立即就清晰了，即
    當數組為 1，2，3，4，.. i，.. n時，基於以下原則的BST建樹具有唯一性：
   以i為根節點的樹，其左子樹由[1, i-1]構成， 其右子樹由[i+1, n]構成。 

」 

```java
public class Solution {
    /**
     * @paramn n: An integer
     * @return: An integer
     */
    public int numTrees(int n) {
        
        if (n <= 1) {
            return 1;
        }
        
        int[] num = new int[n + 1];
        num[0] = 1; // 沒有半個點，只有一種可能
        num[1] = 1; // 只有一點root，只有一種可能
        
        for (int i = 2; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                num[i] += num[j] * num[i - j - 1];
            }
        }
        
        return num[n];
    }
}

```




---
###Reference
1. http://www.cnblogs.com/springfor/p/3884009.html
2. http://fisherlei.blogspot.com/2013/03/leetcode-unique-binary-search-trees.html
3. http://blog.csdn.net/linhuanmars/article/details/24761459
