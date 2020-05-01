# 50. Pow(x, n)

## Problem Description

Implement pow(x,n), which calculates *x* raised to the power *n* (x^n).

## Solutions

### 1. Double myPow

~~~
    double myPow(double x, int n) {
        if(n == 0) return double(1);
        if(n == 1) return x;
        double tmp = myPow(x, n/2);
        if(n % 2) 
        {
            if(n < 0)
                return 1/x * tmp * tmp;
            else
                return x * tmp * tmp;
            
        }
        else
            return tmp * tmp;
        }
~~~

### 2. Double x

~~~
    double myPow(double x, int n) {
        if(n == 0) return 1;
        if(n == 1) return x;
        long nl = n;
        if(n < 0)
        {
            nl = -1 * nl;
            x = 1/x;
        }
        if(n % 2)
            return x * myPow(x * x, nl / 2);
        else
            return myPow(x * x, nl / 2);
     }
~~~