#Search for a Range
[原題網址](http://www.lintcode.com/en/problem/search-for-a-range/)

>解法：

updated on 2016.1.13

下面的程式碼太長了，我們使用不斷的作binary search來作，找下限，就是不斷的把right往左推，找上限就是把left往右推，其程式碼如下：

>此法複雜度較複雜，如果target有n個重複的數，我們需要作n+1次的binary search。

```java
public int[] searchRange(int[] A, int target) {
    int index = binarySearch(A, 0, A.length-1, target);
    int[] result = {-1, -1};
    if (index != -1) {
        int left  = index;
        int right = index;
        result[0] = left;
        result[1] = right;
        while ((left  = binarySearch(A, 0, left-1, target)) != -1)           result[0] = left;
        while ((right = binarySearch(A, right+1, A.length-1, target)) != -1) result[1] = right;
    }
    return result;
}

private int binarySearch(int[] A, int lo, int hi, int target) {
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if      (A[mid] < target) lo = mid + 1;
        else if (A[mid] > target) hi = mid - 1;
        else return mid;            
    }
    return -1;
}
```




因有可能有多個相同的值，需跑兩次二分搜尋找上下限，第一次找目標值的第一個發生的位置，第二次找目標值最後發生的位置。

找下標：當 A[mid] == target 時，把 end 指向 mid 繼續往前搜尋可能相同的值，assign 下標時，記得先從 start開始比，因為我們要找目標值的第一個發生的位置。

找上標：當 A[mid] == target 時，把 start 指向 mid 繼續往後搜尋可能相同的值，assign 上標時，記得先從 end 開始比，因為我們要找目標值最後發生的位置。

```java
public ArrayList<Integer> searchRange(ArrayList<Integer> A, int target) {
        
    int start;
    int end;
    int mid; 
    int midValue;
    int[] range = {-1, -1};
    
    if (A.size() == 0) {
        //you can not direct pass array as paramemter to constructor
        return new ArrayList<Integer>(Arrays.asList(range[0], range[1]));
    }
    
    
    //search lower bound
    start = 0;
    end = A.size() - 1;
    while (start + 1 < end) {
        mid = start + (end - start) / 2;
        midValue = A.get(mid);
        if (midValue == target) {
            //move end ahead since you need to find the first occur element
            end = mid;
        } else if (midValue < target) {
            start = mid;
        } else if (midValue > target) {
            end = mid;
        }
    }
    //since you need to find first occur element, detect start first.
    if (A.get(start) == target) {
        range[0] = start;
    } else if (A.get(end) == target) {
        range[0] = end;
    } else {
        //return directly, since you can not find anything
        range[0] = range[1] = -1;
        return new ArrayList<Integer>(Arrays.asList(range[0], range[1]));
    }
    
    //search upper bound
    start = 0;
    end = A.size() - 1;
    
    while (start + 1 < end) {
        mid = start + (end - start) / 2;
        midValue = A.get(mid);
        if (midValue == target) {
            // move start backward since you need to find the last occur element
            start = mid;
        } else if (midValue < target) {
            start = mid;
        } else {
            end = mid;
        }
    }
    // detect end first, since you need to find the last occur element.
    if (A.get(end) == target) {
        range[1] = end;
    } else if (A.get(start) == target) {
        range[1] = start;
    } else {
        range[1] = range[0] = -1;
        return new ArrayList<Integer>(Arrays.asList(range[0], range[1]));
    }
    
    return new ArrayList<Integer>(Arrays.asList(range[0], range[1]));
}
```

>Time Complexity：O(NlogN)


---
###Reference 
1. https://leetcode.com/discuss/529/the-elements-the-whole-array-the-same-the-target-can-logn-time