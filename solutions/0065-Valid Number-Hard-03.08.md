# Valid Number

### Problem Description

Validate if a given string can be interpreted as a decimal number.

Some examples:
`"0"` => `true`
`" 0.1 "` => `true`
`"abc"` => `false`
`"1 a"` => `false`
`"2e10"` => `true`
`" -90e3  "` => `true`
`" 1e"` => `false`
`"e3"` => `false`
`" 6e-1"` => `true`
`" 99e2.5 "` => `false`
`"53.5e93"` => `true`
`" --6 "` => `false`
`"-+3"` => `false`
`"95a54e53"` => `false`

**Note:** It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one. However, here is a list of characters that can be in a valid decimal number:

- Numbers 0-9
- Exponent - "e"
- Positive/negative sign - "+"/"-"
- Decimal point - "."

Of course, the context of these characters also matters in the input.

**Update (2015-02-10):**
The signature of the `C++` function had been updated. If you still see your function signature accepts a `const char *` argument, please click the reload button to reset your code definition.

### Some Notes

This problem does not state the requirement clearly. Here we show several test samples to help you understand better what is called a "valid number". Credit to [this discussion.](https://leetcode.com/problems/valid-number/discuss/23741/The-worst-problem-i-have-ever-met-in-this-oj)

```Python
test(1, "123", true);
test(2, " 123 ", true);
test(3, "0", true);
test(4, "0123", true);  //Cannot agree
test(5, "00", true);  //Cannot agree
test(6, "-10", true);
test(7, "-0", true);
test(8, "123.5", true);
test(9, "123.000000", true);
test(10, "-500.777", true);
test(11, "0.0000001", true);
test(12, "0.00000", true);
test(13, "0.", true);  //Cannot be more disagree!!!
test(14, "00.5", true);  //Strongly cannot agree
test(15, "123e1", true);
test(16, "1.23e10", true);
test(17, "0.5e-10", true);
test(18, "1.0e4.5", false);
test(19, "0.5e04", true);
test(20, "12 3", false);
test(21, "1a3", false);
test(22, "", false);
test(23, "     ", false);
test(24, null, false);
test(25, ".1", true); //Ok, if you say so
test(26, ".", false);
test(27, "2e0", true);  //Really?!
test(28, "+.8", true);  
test(29, " 005047e+6", true);  //Damn = =|||
```

### Solution 0: Regular Expression

If you understand examples above, you can directly write a regular expression to match this string. More information about regular expression [on this site](https://www.runoob.com/python/python-reg-expressions.html).

A regular expression solution in Java:

```java
class Solution {
    public boolean isNumber(String s) {
        return s.trim().matches("[-+]?(\\d+\\.?|\\.\\d+)\\d*(e[-+]?\\d+)?");
    }
}
```

But this way is so tricky that I list it as solution 0.

### Solution 1: A straight forward solution

This solution is based on analyzing the string case by case. Use flags to determine which situation and whether it is valid.

For example, a solution from [discussion](https://leetcode.com/problems/valid-number/discuss/23738/Clear-Java-solution-with-ifs) (Java):

We start with trimming.

- If we see `[0-9]` we reset the number flags.
- We can only see `.` if we didn't see `e` or `.`.
- We can only see `e` if we didn't see `e` but we did see a number. We reset numberAfterE flag.
- We can only see `+` and `-` in the beginning and after an `e`
- any other character break the validation.

At the and it is only valid if there was at least 1 number and if we did see an `e` then a number after it as well. So basically the number should match this regular expression:

`[-+]?(([0-9]+(.[0-9]*)?)|.[0-9]+)(e[-+]?[0-9]+)?`

```java
public boolean isNumber(String s) {
    s = s.trim();

    boolean pointSeen = false;
    boolean eSeen = false;
    boolean numberSeen = false;
    boolean numberAfterE = true;
    for(int i=0; i<s.length(); i++) {
        if('0' <= s.charAt(i) && s.charAt(i) <= '9') {
            numberSeen = true;
            numberAfterE = true;
        } else if(s.charAt(i) == '.') {
            if(eSeen || pointSeen) {
                return false;
            }
            pointSeen = true;
        } else if(s.charAt(i) == 'e') {
            if(eSeen || !numberSeen) {
                return false;
            }
            numberAfterE = false;
            eSeen = true;
        } else if(s.charAt(i) == '-' || s.charAt(i) == '+') {
            if(i != 0 && s.charAt(i-1) != 'e') {
                return false;
            }
        } else {
            return false;
        }
    }

    return numberSeen && numberAfterE;
}
```

There is another solution which just return False to any inapplicable strings and return True finally. Code is from this [discussion](https://leetcode.com/problems/valid-number/discuss/23762/A-simple-solution-in-cpp) 

```c++
bool isNumber(const char *s) 
{
    int i = 0;

    // skip the whilespaces
    for(; s[i] == ' '; i++) {}

    // check the significand
    if(s[i] == '+' || s[i] == '-') i++; // skip the sign if exist

    int n_nm, n_pt;
    for(n_nm=0, n_pt=0; (s[i]<='9' && s[i]>='0') || s[i]=='.'; i++)
        s[i] == '.' ? n_pt++:n_nm++;       
    if(n_pt>1 || n_nm<1) // no more than one point, at least one digit
        return false;

    // check the exponent if exist
    if(s[i] == 'e') {
        i++;
        if(s[i] == '+' || s[i] == '-') i++; // skip the sign

        int n_nm = 0;
        for(; s[i]>='0' && s[i]<='9'; i++, n_nm++) {}
        if(n_nm<1)
            return false;
    }

    // skip the trailing whitespaces
    for(; s[i] == ' '; i++) {}

    return s[i]==0;  // must reach the ending 0 of the string
}
```

### Solution 2: Deterministic Finite Automaton (DFA)

 The illustration is based on this (credit to [this blog](https://leetcode.wang/leetCode-65-Valid-Number.html)):

![img](https://windliang.oss-cn-beijing.aliyuncs.com/65_2.jpg)

We can implement this graph where orange node represents acceptable state. Code is referenced to [this discussion](https://leetcode.com/problems/valid-number/discuss/23728/A-simple-solution-in-Python-based-on-DFA):

```python
class Solution(object):
    def isNumber(self, s):
        """
        :type s: str
        :rtype: bool
        """
        #define DFA state transition tables
        states = [{},
                 # State (1) - initial state (scan ahead thru blanks)
                 {'blank': 1, 'sign': 2, 'digit':3, '.':4},
                 # State (2) - found sign (expect digit/dot)
                 {'digit':3, '.':4},
                 # State (3) - digit consumer (loop until non-digit)
                 {'digit':3, '.':5, 'e':6, 'blank':9},
                 # State (4) - found dot (only a digit is valid)
                 {'digit':5},
                 # State (5) - after dot (expect digits, e, or end of valid input)
                 {'digit':5, 'e':6, 'blank':9},
                 # State (6) - found 'e' (only a sign or digit valid)
                 {'sign':7, 'digit':8},
                 # State (7) - sign after 'e' (only digit)
                 {'digit':8},
                 # State (8) - digit after 'e' (expect digits or end of valid input) 
                 {'digit':8, 'blank':9},
                 # State (9) - Terminal state (fail if non-blank found)
                 {'blank':9}]
        currentState = 1
        for c in s:
            # If char c is of a known class set it to the class name
            if c in '0123456789':
                c = 'digit'
            elif c in ' \t\n':
                c = 'blank'
            elif c in '+-':
                c = 'sign'
            # If char/class is not in our state transition table it is invalid input
            if c not in states[currentState]:
                return False
            # State transition
            currentState = states[currentState][c]
        # The only valid terminal states are end on digit, after dot, digit after e, or white space after valid input    
        if currentState not in [3,5,8,9]:
            return False
        return True
```

### Solution 3: Design Pattern

This solution is based on [this discussion](https://leetcode.com/problems/valid-number/discuss/23977/A-clean-design-solution-By-using-design-pattern). The code is more extendable than other solution. It builds  classes for verify different types of numbers (exponential, decimal points, only numbers...). This code is easy to extend, debug and read. 

```c++
//每个类都实现这个接口
interface NumberValidate { 
    boolean validate(String s);
}
//定义一个抽象类，用来检查一些基础的操作，是否为空，去掉首尾空格，去掉 +/-
//doValidate 交给子类自己去实现
abstract class  NumberValidateTemplate implements NumberValidate{

    public boolean validate(String s)
    {
        if (checkStringEmpty(s))
        {
            return false;
        }

        s = checkAndProcessHeader(s);

        if (s.length() == 0)
        {
            return false;
        }

        return doValidate(s);
    }

    private boolean checkStringEmpty(String s)
    {
        if (s.equals(""))
        {
            return true;
        }

        return false;
    }

    private String checkAndProcessHeader(String value)
    {
        value = value.trim();

        if (value.startsWith("+") || value.startsWith("-"))
        {
            value = value.substring(1);
        }


        return value;
    }



    protected abstract boolean doValidate(String s);
}

//实现 doValidate 判断是否是整数
class IntegerValidate extends NumberValidateTemplate{

    protected boolean doValidate(String integer)
    {
        for (int i = 0; i < integer.length(); i++)
        {
            if(Character.isDigit(integer.charAt(i)) == false)
            {
                return false;
            }
        }

        return true;
    }
}

//实现 doValidate 判断是否是科学计数法
class SienceFormatValidate extends NumberValidateTemplate{

    protected boolean doValidate(String s)
    {
        s = s.toLowerCase();
        int pos = s.indexOf("e");
        if (pos == -1)
        {
            return false;
        }

        if (s.length() == 1)
        {
            return false;
        }

        String first = s.substring(0, pos);
        String second = s.substring(pos+1, s.length());

        if (validatePartBeforeE(first) == false || validatePartAfterE(second) == false)
        {
            return false;
        }


        return true;
    }

    private boolean validatePartBeforeE(String first)
    {
        if (first.equals("") == true)
        {
            return false;
        }

        if (checkHeadAndEndForSpace(first) == false)
        {
            return false;
        }

        NumberValidate integerValidate = new IntegerValidate();
        NumberValidate floatValidate = new FloatValidate();
        if (integerValidate.validate(first) == false && floatValidate.validate(first) == false)
        {
            return false;
        }

        return true;
    }

    private boolean checkHeadAndEndForSpace(String part)
    {

        if (part.startsWith(" ") ||
            part.endsWith(" "))
        {
            return false;
        }

        return true;
    }

    private boolean validatePartAfterE(String second)
    {
        if (second.equals("") == true)
        {
            return false;
        }

        if (checkHeadAndEndForSpace(second) == false)
        {
            return false;
        }

        NumberValidate integerValidate = new IntegerValidate();
        if (integerValidate.validate(second) == false)
        {
            return false;
        }

        return true;
    }
}
//实现 doValidate 判断是否是小数
class FloatValidate extends NumberValidateTemplate{

    protected boolean doValidate(String floatVal)
    {
        int pos = floatVal.indexOf(".");
        if (pos == -1)
        {
            return false;
        }

        if (floatVal.length() == 1)
        {
            return false;
        }

        NumberValidate nv = new IntegerValidate();
        String first = floatVal.substring(0, pos);
        String second = floatVal.substring(pos + 1, floatVal.length());

        if (checkFirstPart(first) == true && checkFirstPart(second) == true)
        {
            return true;
        }

        return false;
    }

    private boolean checkFirstPart(String first)
    {
        if (first.equals("") == false && checkPart(first) == false)
        {
            return false;
        }

        return true;
    }

    private boolean checkPart(String part)
    {
        if (Character.isDigit(part.charAt(0)) == false ||
            Character.isDigit(part.charAt(part.length() - 1)) == false)
        {
            return false;
        }

        NumberValidate nv = new IntegerValidate();
        if (nv.validate(part) == false)
        {
            return false;
        }

        return true;
    }
}
//定义一个执行者，我们把之前实现的各个类加到一个数组里，然后依次调用
class NumberValidator implements NumberValidate {

    private ArrayList<NumberValidate> validators = new ArrayList<NumberValidate>();

    public NumberValidator()
    {
        addValidators();
    }

    private  void addValidators()
    {
        NumberValidate nv = new IntegerValidate();
        validators.add(nv);

        nv = new FloatValidate();
        validators.add(nv);

        nv = new HexValidate();
        validators.add(nv);

        nv = new SienceFormatValidate();
        validators.add(nv);
    }

    @Override
    public boolean validate(String s)
    {
        for (NumberValidate nv : validators)
        {
            if (nv.validate(s) == true)
            {
                return true;
            }
        }

        return false;
    }


}
public boolean isNumber(String s) {
    NumberValidate nv = new NumberValidator(); 
    return nv.validate(s);
}

```

 ### Last Words

This problem is annoying. However, we can learn a bit from it, like DFA or Design Pattern. Solutions are basically based on [this blog.](https://leetcode.wang/leetCode-65-Valid-Number.html) You check out his blog as well. 