[Metadata]
title: Fraction to Recurring Decimal 
date: 2015-07-16 01:09:00

[Tags]
difficulty: 3.5
categories: math
source: Leetcode, anonymous

[Description]

Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.

If the fractional part is repeating, enclose the repeating part in parentheses.

For example,

* Given numerator = 1, denominator = 2, return "0.5".
* Given numerator = 2, denominator = 1, return "2".
* Given numerator = 2, denominator = 3, return "0.(6)".

[Solution]

It's great that there are several test cases in the description. But it's far from enough for us to solve this problem.

There are always some pitfalls for some "easy" problems in the interview. The crux to deal with it is to ask the interviewer what is **really** want for this one.

Here is some corner cases that we have to deal with:

* numerator = 0, denominator = 2, return "0"
* numerator = 1, denominator = 0, invalid test case
* numerator = 1, denominator = 333, return "0.(003)"
* numerator = -1, denominator = 2, return "-0.5"
* numerator = 1, denominator = -3, return "-0.(3)"
* numerator = -2147483648, denominator = -1, return "2147483648", be ware that the result is beyond the range of a integer

Other than write this cases out, you should run the "code" in your mind before you write it down on the paper or whiteboard. It's essential that you can change your mind easier than the code on the paper. Sometimes you just keep patching your written code, and eventually find out that the code becomes a mess and error-prone.

Remember, case first, brain emulation second, code always comes the last.

```cpp
class Solution {
    typedef long long llint;
public:
    string fractionToDecimal(llint numerator, llint denominator) {
        int sig = 1;
        if ((numerator < 0) ^ (denominator < 0)) {
            sig = -1;
        }
        numerator   = abs(numerator);
        denominator = abs(denominator);
        
        string a = to_string(numerator / denominator);
        string b = do_frac_to_dec(numerator % denominator, denominator);
        if (a == "0" && b.empty()) {
            return "0";
        }
        if (sig < 0) {
            a = "-" + a;
        }
        if (b.empty()) {
            return a;
        } else {
            return a + "." + b;
        }
    }
private:
    string do_frac_to_dec(llint a, llint b) {
        unordered_map<llint, llint> mp;
        vector<llint> vec;
        
        int ptr = -1;
        while (a) {
            if (mp.find(a) != mp.end()) {
                ptr = mp[a];
                break;
            } else {
                mp[a] = vec.size();
            }
            a *= 10;
            vec.push_back(a / b);
            a %= b;
        }
        
        string res;
        for (size_t i = 0; i < vec.size(); i++) {
            if (i == ptr) {
                res += "(";
            }
            res += to_string(vec[i]);
        }
        if (ptr != -1) {
            res += ")";
        }
        return res;
    }
};
```
