### [LeetCode 1073](https://leetcode.com/problems/adding-two-negabinary-numbers/) Adding Two Negabinary Numbers

First, let's talk about the basis of this problem: [LeetCode 1071](https://leetcode.com/problems/convert-to-base-2/).

Lee has given us the solution ([reference link](https://leetcode.com/problems/convert-to-base-2/discuss/265507/JavaC%2B%2BPython-2-lines-Exactly-Same-as-Base-2)).

He began with base 2 and then modified it to fit the requirement of base -2.

```java
public String base2(int N) {
    StringBuilder res = new StringBuilder();
    while (N != 0) {
        res.append(N & 1); //check last digit by N % 2 or N & 1
        N = N >> 1; 
        //This actually do two things: minus 1 if last digit is 1; divide N by 2.
    }
    return res.length() > 0 ? res.reverse().toString() : "0";
}

public String baseNeg2(int N) {
    StringBuilder res = new StringBuilder();
    while (N != 0) {
        res.append(N & 1);
        N = -(N >> 1);
    }
    return res.length() > 0 ? res.reverse().toString() : "0";
}
```

When considering base -2, note that they are **different** (in Java)

```java
int N = -1;
System.out.println(-(N >> 1));
System.out.println(N/(-2));
```

Output:
`1`
`0`

C++ and Python have the same results.

If we want to convert the value into base -2, we should follow the rule that the remainder is positive. Therefore, we prefer -(N >> 1) to N/(-2).

Then, we come back to LeetCode 1073. The idea is similar to 1071. Note that the addition is from backward to forward. Finally, we use a stack to reverse the array.

```java
public int[] addNegabinary(int[] arr1, int[] arr2) {
    int carry = 0;
    int i = arr1.length - 1, j = arr2.length - 1;
    Stack<Integer> stack = new Stack<>();
    while (i >= 0 || j >= 0 || carry != 0) {
        if (i >= 0) carry += arr1[i--];
        if (j >= 0) carry += arr2[j--];
        stack.push(carry & 1);
        carry = -(carry >> 1);
    }
    while (!stack.isEmpty() && stack.peek() == 0) {
        stack.pop(); // remove 0 at the beginning of the array
    }
    int[] res = new int[stack.size()];
    int index = 0;
    while (!stack.isEmpty()) {
        res[index++] = stack.pop();
    }
    return res.length == 0 ? new int[1] : res;
}
```



