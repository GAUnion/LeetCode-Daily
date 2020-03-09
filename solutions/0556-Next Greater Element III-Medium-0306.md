### Next Greater Element 3 ###

------

#### Problem

Given a positive 32-bit integer n, you need to find the smallest 32-bit integer which has exactly the same digits existing in the integer n and is greater in value than n. If no such positive 32-bit integer exists, you need to return -1.

#### Solution 1:  List all possibilities and compare

------

Time complexity: O(n!)

Space complexity: O(n!)

Tooooo violent that let's ignore it.

#### Solution 2: Linear

------

Pre-process - Turn int to digits in the same order.

```C++
vector<int> digit;
while (n != 0) {
    digit.insert(digit.begin(),n%10);
    n/=10;
}
```

From right to left, find the first digit that's smaller than the previous one. Set position as idx.

From idx to right, find the least larger digit than digit[idx]. Use *swap()* to exchange them.

For digits on the right side of digit[idx], use *sort()* to make them ascending. 

```c++
if(n > INT_MAX) return -1;
int idx = -1;
for(int i = digit.size()-1;i > 0;--i) {
    if(digit[i-1] < digit[i]) {
        idx = i-1;
        break;
    }
}
if(idx == -1) return -1;

int min = idx + 1;
for(int i = idx + 2; i < digit.size();++i) {
    if(digit[i] > digit[idx] && digit[i] <digit[min]) min = i;
}
swap(digit[min],digit[idx]);
sort(digit.begin() + idx + 1, digit.end());
long rst = 0;
for(int i = 0; i < digit.size();++i) {
    rst = rst * 10 + digit[i];
}
return rst > INT_MAX && rst > 0 ? -1 : rst;
    
```

Or if you don't want to use sort, don' swap, get the digit that should be exchanged .Simply reverse the right side of idx, and they're now in ascending order. Then put the  digit in the right place using dichotomy (like *upper_bound()*).

```c++
reverse(digit.begin() + idx + 1, digit.end());
int r = upper_bound(digit.begin() + idx + 1, digit.end(), digit[l]) - digit.begin();
swap(digit[idx], digit[r]);
```

PS: Mention the corner cases: n > INT_MAX, rst > INT_MAX, rst < 0

Time complexity: O(n)

Space complexity: O(n)

#### Solution 3: use STL: next_permuation

------

In-built algorithm to find the next permuation =.=

```c++
int nextGreaterElement(int n) {
    string s=to_string(n);
    if(next_permutation(s.begin(),s.end())){
        long long r= stoll(s);
        if(r > INT_MAX || r <INT_MIN) return -1;
        else return r;
    }else{
        return -1;
    }
}
```