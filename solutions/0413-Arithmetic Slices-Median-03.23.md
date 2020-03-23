# Arithmetic Slices

### Method1: Dynamic Programming


We can have an array dp to store the number of Arithmetic Slices ends with each element of the input. It's obvious that if the interval between a[i] and a[i-1] is different from the interval between a[i-1] and a[i-2], dp[i] should be 0, but if they are equal, dp[i] should be dp[i-1] + 1, because all the Arithmetic Slices ends with a[i-1] can be extend to longer Arithmetic Slices and a[i-2], a[i-1], a[i] can be a new Arithmetic Slice.

```python
class Solution:
    def numberOfArithmeticSlices(self, A: List[int]) -> int:
        dp=[0 for i in range(len(A))]
        dp[0]=0
        dp[1]=0
        for i in range(2,len(A)):
            if A[i]-A[i-1]==A[i-1]-A[i-2]:
                dp[i]=dp[i-1]+1
            else:
                dp[i]=0
        return sum(dp)
```

### Method2: Recursive Method

Similar to the DP method, a top down Recursive Method can be designed. The only difference is that this time you start with the last element of the input.

```python
class Solution:
    def recursive(self, A, end):
        if end==1:
            return 0
        if A[end]-A[end-1]==A[end-1]-A[end-2]:
            return 2*self.recursive(A,end-1)+1
        else:
            return self.recursive(A,end-1)
        
    def numberOfArithmeticSlices(self, A: List[int]) -> int:
        return self.recursive(A,len(A)-1)
```

