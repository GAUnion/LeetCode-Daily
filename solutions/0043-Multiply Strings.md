# Problem
Given two non-negative integers num1 and num2represented as strings, return the product of num1 and num2, also represented as a string.

# Solution
就是个高精度嘛(x

A standard manual multiplication algorithm. Similar to how we learn multiplication in primary school, we just sum up all the cross-digit multiplication.

```[C++]
    string multiply(string num1, string num2) {
        int len1 = num1.length(), len2 = num2.length();
        string ans(num1.length()+num2.length(),'0');
        for (int i=len1-1; i>=0; i--)
        {
            int carry=0;
            for (int j=len2-1; j>=0; j--){
                int position = i+j+1;
                int temp = (num1[i]-'0') * (num2[j]-'0') + ans[position]-'0' + carry;
                ans[position] = temp % 10 + '0';
                carry = temp / 10;
                // ans[position-1] = temp/10;
            }
            ans[i] += carry; // This is correct, since the carry_on digit can never greater than 10,
        }
            
        int first_digit = ans.find_first_not_of("0"); 
        if (first_digit == -1) return "0";
                        else return ans.substr(first_digit);
    }
```

This solution is very concise. However, I think that if the base is 1000 rather than 10, it can run faster as long as the numbers are large. I will do this experiment later.