#Product of Array Exclude itself

[原題連結](http://www.lintcode.com/en/problem/product-of-array-exclude-itself/)

題意：算出陣列的總積除了當下的那個元素i，不能使用除法。


Given an integers array A.

Define ```B[i] = A[0] * ... * A[i-1] * A[i+1] * ... * A[n-1]```, calculate B WITHOUT divide operation.

For ```A = [1, 2, 3]```, return ```[6, 3, 2]```.


解題思路：分別用兩個陣列 left 與 right 
+ $$left[i]$$ 紀錄從0乘到 i - 1 的總積
+ $$right[i]$$ 紀錄從 i + 1 乘到 len - 1的總積
+ $$left[i] * right[i]$$ 代表從頭乘到尾，但排除A[i]
+ 此方式需要花 $$O(2N)$$的空間複雜度

程式碼如下：

```java
public ArrayList<Long> productExcludeItself(ArrayList<Integer> A) {
    
    ArrayList<Long> res = new ArrayList<Long>();
    
    if (A == null || A.size() == 0) {
        return res;
    }
    
    // 利用left與right的兩個陣列
    // left[i]代表從A[0] * ... * A[i-1]的積
    // right[i] 代表從A[i+1] * ... * A[len - 1]的積
    int len = A.size();
    long[] left = new long[len];
    long[] right = new long[len];
    long productLeft = 1;
    long productRight = 1;
    
    for (int i = 0; i < len; i++) {
        productLeft *= (i > 0) ? A.get(i-1) : 1;
        left[i] = productLeft;
        productRight *= (i > 0) ? A.get(len - i) : 1;
        right[len - i - 1] = productRight;
    }
    
    // left[i] * right[i] 代表整個整陣的總積除了第i個元素外
    for (int i = 0; i < len; i++) {
        res.add(left[i] * right[i]);
    }
    
    return res;
    
}
```

---
另有一 $$O(1)$$ 空間複雜度的作法，直接在原本的陣列中作

程式碼如下，需要注意的是要使用 long long 否則會出現 overflow：

```java
public ArrayList<Long> productExcludeItself(ArrayList<Integer> A) {
        
    ArrayList<Long> res = new ArrayList<Long>();
    
    if (A == null || A.size() == 0) {
        return res;
    }
    
    int len = A.size();
    res.add(1);
    
    for (int i = 1; i < len; i++) {
        res.add( res.get(i - 1) * A.get(i - 1));
    }
    
    // 因怕值會被蓋掉，因此使用一個temp來保存目前的乘積
    int temp = 1;
    for (int i = len - 1; i >= 0; i--) {
        res.set(i, res.get(i) * temp);
        temp *= A.get(i);
    }
    
    return res;
    
}
```

updated on 2016.1.9

需要O(N) space

```java
public class Solution {
    public int[] productExceptSelf(int[] nums) {
        
        if (nums == null || nums.length == 0) {
            return new int[0];
        }
        
        int len = nums.length;
        int[] left = new int[len];
        int[] right = new int[len];
        
        left[0] = 1;
        for (int i = 1; i < left.length; i++) {
            left[i] = left[i - 1] * nums[i - 1];
        }
        
        right[len - 1] = 1;
        for (int i = len - 2; i >= 0; i--) {
            right[i] = right[i + 1] * nums[i + 1];
        }
        
        int[] res = new int[len];
        for (int i = 0; i < len; i++) {
            res[i] = left[i] * right[i];
        }
        
        return res;
    }
}
```

O(1) Space

```java
public int[] productExceptSelf(int[] nums) {

    int len = nums.length;
    int [] output = new int[len];

    int leftMult = 1, rightMult = 1;

    for(int i = len-1; i >= 0; i--){
        output[i] = rightMult;
        rightMult *= nums[i];
    }
    for(int j = 0; j < len; j++){
        output[j] *= leftMult;
        leftMult *= nums[j];

    }

    return output; 

}
```

>credit to : http://algorithm.yuanbin.me/zh-cn/integer_array/product_of_array_exclude_itself.html