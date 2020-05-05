# Number of Digit One

### Method : 

给出N的一般规律，比如N=abced五位数字的时候，我们分析百位c，有三种情况：

1）c == 0的时候，比如12013，此时百位出现1的是：00 100 ~ 00 199， 01 100~01 199，……，11 100~ 11 199，共1200个，显然这个有高位数字决定，并且受当前位数影响；

2）c == 1的时候，比如12113，此时百位出现1的肯定包括c=0的情况，另外还需要考虑低位的情况即：00100 ~ 00113共114个，

3）c >= 2的时候，比如12213，此时百位出现1的是：00 100 ~ 00 199， 01 100~01 199，……，11 100~ 11 199，12 100 ~ 12 199，共1300个，这个有高位数字决定，其实是加一，并且乘以当前位数

```java
class Solution {
    public int countDigitOne(int n) {
        if (n <= 0) {
            return 0;
        }
        long count = 0;
        long factor = 1;
        
        while (n / factor != 0) {
            long lowNum = n - (n / factor) * factor;
            long currNum = (n / factor) % 10;
            long highNum = n / (factor * 10);
            
            if (currNum == 0) {
                count += highNum * factor;
            } else if (currNum == 1) {
                count += highNum * factor + lowNum + 1;
            } else {
                count += (highNum + 1) * factor;
            }
            
            factor *= 10;
        }
        return (int)count;
    }
}
```
