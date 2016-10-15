# Read N Characters Given Read4

[Leetcode](https://leetcode.com/problems/read-n-characters-given-read4/)


題意：要讀進一段文字，但只能用一個function 一次讀4個char


解題思路：


```java
/* The read4 API is defined in the parent class Reader4.
      int read4(char[] buf); */

public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Maximum number of characters to read
     * @return    The number of characters read
     */
    public int read(char[] buf, int n) {
        int totalRead = 0;
        char[] buffer = new char[4];
        
        while(true) {
            int currentCount = read4(buffer);
            int currentLength = Math.min(currentCount, n - totalRead);
            for (int i = 0; i < currentLength; i++) {
                buf[totalRead + i] = buffer[i];
            }
            
            totalRead += currentLength;
            if (currentCount < 4 || totalRead == n) {
                return totalRead;
            }
        }
    }
}
```