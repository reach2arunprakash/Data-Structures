#Cosine Similarity

[原題網址](http://www.lintcode.com/en/problem/cosine-similarity/)

題意：

Cosine similarity is a measure of similarity between two vectors of an inner product space that measures the cosine of the angle between them. The cosine of 0° is 1, and it is less than 1 for any other angle.

See wiki: [Cosine Similarity](https://en.wikipedia.org/wiki/Cosine_similarity)

Given two vectors A and B with the same size, calculate the cosine similarity.

Return 2.0000 if cosine similarity is invalid (for example A = [0] and B = [0]).

Have you met this question in a real interview? Yes
Example
Given A = [1, 2, 3], B = [2, 3 ,4].

Return 0.9926.

Given A = [0], B = [0].

Return 2.0000

解題思路：

水題，照著 wiki上的公式直接寫即可，程式碼如下：

```java
class Solution {
    /**
     * @param A: An integer array.
     * @param B: An integer array.
     * @return: Cosine similarity.
     */
    public double cosineSimilarity(int[] A, int[] B) {
        
        if (A.length == 0 || A.length == 1 && A[0] == B[0]) {
            return 2.0;
        }
        
        double productSum = 0;
        double aSquare = 0;
        double bSquare = 0;
        for (int i = 0; i < A.length; i++) {
            productSum += A[i] * B[i];
            aSquare += A[i] * A[i];
            bSquare += B[i] * B[i];
        }
        
        double aSquareRoot = Math.sqrt(aSquare);
        double bSquareRoot = Math.sqrt(bSquare);
        if (aSquareRoot * bSquareRoot != 0) {
            return productSum / (aSquareRoot * bSquareRoot);
        } else {
            return 2.0;
        }
        
    }
}


```