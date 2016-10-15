#Subarray Sum Closest

[]()

題意：Given an integer array, find a subarray with sum closest to zero. Return the indexes of the first number and last number.

Given [-3, 1, 1, -3, 5], return [0, 2], [1, 3], [1, 1], [2, 2] or [0, 4]

解題思路：

1. 建立一個 pair class，sum代表前i個陣列元素和，index表是前前i個
2. 遍歷一次陣列求得子陣列和，以 sums 陣列來存，sums存的是一個 pair。

    sums = { (0, 0), (-3, 1), (-2, 2), (-1, 3), (-4, 4), (1, 5) }
3. 將此陣列以元素和來作排序，結果如下

    sums = { (-4, 4), (-3,1), (-2, 2), (-1,3), (0,0), (1, 5) }
    
4. 經過排序後，相近sum的index皆已在附近了，因此只需要兩兩作比較即可，但因為排序過後，index的前後可能已被交換了，因此在更新答案時，需要根據index排序。

    由於這題範例兩兩皆差一，因此以下皆為答案，我們取(4, 1)
    { (4, 1), (1, 2), (2, 3), (3, 0), (0, 5) }

    (4, 1) --轉index--> (3, 0) --index排序--> (0, 3) --修正第一個index--> (1,3)
    
    (1, 3) 即為我們要找的答案 1 + 1 + (-3) = -1
>[注意] 因sums[i]的i代表的是前i個元素個數，因此在轉回index時，需要減1，sums[i].index 也是如此。


```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number 
     *          and the index of the last number
     */
     
    class Pair {
        int sum;
        int index;
        public Pair(int s, int i) {
            this.sum = s;
            this.index = i;
        }
    }
    
    public ArrayList<Integer> subarraySumClosest(int[] nums) {
        
        ArrayList<Integer> res = new ArrayList<Integer>();
        
        if (nums == null || nums.length == 0) {
            return res;
        }
        
        int len = nums.length;
        
        //只有一個元素，前後 index 只能是自己
        if (len == 1) {
            res.add(0);
            res.add(0);
            return res;
        }
        
        // Pair[i] 表示前i個元素的元素和，與前i個元素個數。
        Pair[] sums = new Pair[len + 1];
        int prev = 0;
        sums[0] = new Pair(0, 0);
        
        // 先計算出所有的元素和，與紀錄index
        for (int i = 1; i <= len; i++) {
            sums[i] = new Pair(prev + nums[i-1], i);
            prev += nums[i - 1];
        }
        
        // 對於陣列以元素和作排序，因是自定的 pair class，因此得實作 comparator
        Arrays.sort(sums, new Comparator<Pair>() {
           public int compare(Pair a, Pair b) {
               return a.sum - b.sum;
           } 
        });
        
        // 因陣列已根據元素和排序過，因此只需要找出兩兩之間最小差的值即可
        int ans = Integer.MAX_VALUE;
        for (int i = 1; i <= len; i++) {
            
            if (ans > sums[i].sum - sums[i - 1].sum) {
                // 有新的更接近的值，把原本的結果清掉並更新
                ans = sums[i].sum - sums[i - 1].sum;
                res.clear();
                
                int[] temp = new int[]{sums[i].index - 1, sums[i - 1].index - 1};
                Arrays.sort(temp);
                
                // 因要排除temp[0]之前的元素和，因此將第一個index 加1
                res.add(temp[0] + 1);
                res.add(temp[1]);
            }
        }
        
        return res;
    }
}


```

---
###Reference
1. http://www.jiuzhang.com/solutions/subarray-sum-closest/