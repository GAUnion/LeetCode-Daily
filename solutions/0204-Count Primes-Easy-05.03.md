# 204. Count Primes

## Problem Description

Count the number of prime numbers less than a non-negative number, n.

## Solutions

***
### 1. traversal

Determine one by one whether the number less than n is a prime number.

```c
int countPrimes(int n) {
        int res=1;
        if(n<3) return 0;
        else 
        {
            for(int i=3;i<n;i++)
            {
                if(i%2==0)
                    continue;
                bool flag=true;
                for(int j=3;j<=sqrt(i);j+=2)
                {
                    if(i%j==0)
                    {
                        flag=false;
                        break;
                    }
                }
                if(flag)
                {
                    res++;
                }
            }
        }
        return res;
    }
```

***
### 2. Sieve of Eratosthenes

```c++
 int countPrimes(int n) {
        vector<bool> dp(n, true);
        for (int i=2; i<n; i++) {
            if (dp[i]) {
                for (int j = 2; i * j < n; j++) {
                    dp[i*j] = bbfalse;
                }
            }
        }
        int count = 0;
        for (int i=2; i<n; i++) {
            if (dp[i]) count++;
        }
        return count;
    }
```

