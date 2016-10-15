# Perfect Number

[]()


題意：

找出特定區間的所有perfect number，perfect number的定義是所有質因數相加是否等於原本的數。


解題思路：


```java
import java.io.IOException;

// Finding perfect numbers in a range using Java
public class PerfectNumbersInRange {

    public static void main(String[] args) throws IOException {
        int starting_number = 1;
        int ending_number = 10000;
        System.out.println("Perfect Numbers between "+starting_number+ " and "+ending_number);
        for (int i = starting_number; i <= ending_number; i++) {
            if (isPerfectNumber(i)) {
                System.out.println(i+" is a perfect number");
            } 
        }
    }
    
    /**
     * Checks whether the given number is a perfect number
     * @param number
     * @return 
     */
    public static boolean isPerfectNumber(int number) {
        int sum=0;         
        for(int i=1; i<=number/2; i++) {
            if(number%i == 0) {
                sum += i;
            }
        }
         
        if(sum==number) { 
            return true;
        }else {
            return false;
        }
    }
 
}
```