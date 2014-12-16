[Metadata]
title: Divide Number
date: 2014-12-17 00:31:19 

[Tags]
difficulty: 2.5
categories: math, hash, string
source: mitbbs, google

[Description]

Divide number and return result in form of a string.

For example, `100/3` result should be `33.(3)`. Here 3 is in brackets because it gets repeated continuously. And `5/10` should be `0.5`.

[Solution]

The integer part of the final solution is easy to get, that is, ``to_string(int(a / b))``. The hard part is to find the fraction part, and find the loop in it.

If the result of `a / b` is not circulating decimal, we can get the final result step by step.

```python
def divide(a, b):
    int_part = str(a / b)
    frac_part = ''
    
    a = a % b * 10
    while a:
        c = a / b
        frac_part += str(c)
        a = (a % b) * 10
    print int_part + '.' + frac_part
```

But if we come across a circulating one, our algorithm will looping infinitely. All we have to do, is to find the begining and the end of the loop to make the algorithm stop.

Consider this scenario, $a_i$ stands for the value of a in the ith iteration of the `while` loop in that code above, and $c_i$ stands for the very result digit. So if $a_i = a_j$, we can easily get $c_i = c_j$, and $c_{i+k} = c_{j+k}$. That is, if we get $a_i = a_j$，the number from `i` to `j - 1` will be the **recurring period**.

And here is the final code:

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <string>
#include <vector>
#include <iostream>
#include <algorithm>
#include <unordered_map>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

string make_result(const vector<int>& vec, int idx) {
    string res = "";
    int len = vec.size();
    for (int i = 0; i < idx; i++) {
        res += to_string(vec[i]);
    }
    res += "(";
    for (int i = idx; i < len; i++) {
        res += to_string(vec[i]);
    }
    res += ")";
    return res;
}

string to_frac(int a, int b) {
    unordered_map<int, int> mp;
    vector<int> vec;
    int idx = 0;

    while (true) {
        if (mp.find(a) != mp.end()) {
            return make_result(vec, mp[a]);
        } else {
            vec.push_back(a / b);
            mp[a] = idx;
        }
        a = (a % b) * 10;
        idx++;
    }
}

int main () {
    int a, b;
    while (input(a >> b)) {
        print(">> " << a << " " << b);
        string int_part = to_string(a / b);
        string fac_part = to_frac(a % b * 10, b);
        print(int_part << '.' << fac_part);
    }
    return 0;
}
```
