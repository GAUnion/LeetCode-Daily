# 537. Complex Number Multiplication

## Problem Description

Given two strings representing two [complex numbers](https://en.wikipedia.org/wiki/Complex_number).

You need to return a string representing their multiplication. Note i2 = -1 according to the definition.



## Solution

Find the real part and imaginary part of each complex number, and multiply them separately.

~~~
string complexNumberMultiply(string a, string b) {
  int add_location_a = a.find('+');
  int add_location_b = b.find('+');

  int a1 = atoi(a.substr(0, add_location_a).c_str());
  int b1 = atoi(b.substr(0, add_location_b).c_str());
  int a2 = atoi(a.substr(add_location_a + 1, a.length() - add_location_a - 2).c_str());
  int b2 = atoi(b.substr(add_location_b + 1, b.length() - add_location_b - 2).c_str());

  int result1 = a1*b1 - a2*b2;
  int result2 = a1*b2 + a2*b1;

  return to_string(result1) + "+" + to_string(result2) + "i";

}
~~~

