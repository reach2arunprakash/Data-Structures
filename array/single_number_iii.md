#Single Number III

[原題連結](http://www.lintcode.com/en/problem/single-number-iii/)

題意：給予 $$2N + 2$$ 個元素，每個元素皆出現兩次，但其中兩個元素(假設是a與b)各出現一 次，找出那兩個數。

解題思路：所要思考就是怎麼把 $$2N+2$$ 的問題轉換成兩個 $$2N+1$$ 的問題，當我們把所有元素全 XOR 起來時，其實就等於 ``` c = a ^ b```，而要如何把這兩個元素分開，因他們兩個數不同，所以在二進制的情況下，該位元若等於一，則代表兩數在該位元不同，我們只要利用該位元來將所有元素分成兩堆即可。此時分別將兩堆利用Single Number I的方法來處理即可。

>在這裡我們使用了以下技巧來將最後一個位元1取出來：假設我們XOR的結果為6(1010)，我們要取出(0010)，首先使用```result & (result - 1)```而得到1000，皆著再用原本的result去與剛剛的結果相減，即得到1010-1000 = 0010，為我們需要的數。

```java
public List<Integer> singleNumberIII(int[] A) {
    int len = A.length;
    int result = 0;
    // 先把所有數都xor起來，會得到a^b
    for (int i = 0; i < len; i++) {
        result ^= A[i];
    }
    
    // 接著用這個方法來找出最後一個不一樣的位元
    // 靠這個位元來決定每個數要放到哪個group
    // 等於轉成兩個2n+1的問題來解
    // 之後把這兩個值加到list回傳即可
    // result & (result - 1) 會把最後一個1去掉
    // 再用result去減可以得到最後一個1
    // 接著直接去比這個bit即可
    int lastBit = result - (result & (result - 1));
    int group1 = 0;
    int group2 = 0;

    for (int i = 0; i < len; i++) {
        if ((A[i] & lastBit) == 0) {
            group1 ^= A[i];
        } else {
            group2 ^= A[i];
        }
    } 

    List<Integer> list = new ArrayList<Integer>();
    list.add(group1);
    list.add(group2);
    
    return list;
}
```

>Time Complexity：$$O(N)$$