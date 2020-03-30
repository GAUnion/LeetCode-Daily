## Problem

Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

## Solution

For this problem, we can know that for every int n it can be divided into no more than 4 int.

* If it fits n = (4^a)*(8b+7) then ans is 4
* if it fits n = a^2 + b^2 then ans is 1 or 2
* else ans is 3

```C++
class Solution {
public:
    int numSquares(int n) {
        while(n%4 == 0) n /= 4;
        if(n%8==7) return 4;
        int ans = 0;
        while(ans*ans<=n){
            int b = int(sqrt(n-ans*ans));
            if(ans*ans+b*b==n){
                if(ans == 0 || b == 0) return 1;
                else return 2;
            }
            ans+=1;
        }
        return 3;
    }
};
```
