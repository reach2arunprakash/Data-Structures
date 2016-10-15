# Compare Version Numbers

[Leetcode](https://leetcode.com/problems/compare-version-numbers/)

題意：

Compare two version numbers version1 and version2.
If version1 > version2 return 1, if version1 < version2 return -1, otherwise return 0.

You may assume that the version strings are non-empty and contain only digits and the . character.
The . character does not represent a decimal point and is used to separate number sequences.
For instance, 2.5 is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

Here is an example of version numbers ordering:
```
0.1 < 1.1 < 1.2 < 13.37
```
解題思路：

可用split法，將字串以“。”分成前後部份，比較兩個字串的前後子字串來決定大小。

網友提供了一個不需要split的方法，使用while來讀每個character，其程式碼如下：

```java
public class Solution {
    public int compareVersion(String version1, String version2) {
        int lenOne = version1.length();
        int lenTwo = version2.length();
        int tempOne = 0;
        int tempTwo = 0;
        int idxOne = 0;
        int idxTwo = 0;
        
        while (idxOne < lenOne || idxTwo < lenTwo) {
            tempOne = 0;
            tempTwo = 0;
            while(idxOne < lenOne && version1.charAt(idxOne) != '.') {
                tempOne = tempOne * 10 + version1.charAt(idxOne) - '0';
                idxOne++;
            }
            while(idxTwo < lenTwo && version2.charAt(idxTwo) != '.') {
                tempTwo = tempTwo * 10 + version2.charAt(idxTwo) - '0';
                idxTwo++;
            }
            if (tempOne > tempTwo) {
                return 1;
            } else if (tempOne < tempTwo) {
                return -1;
            } else {
                idxOne++;
                idxTwo++;
            }
        }
        return 0;
    }
}
```