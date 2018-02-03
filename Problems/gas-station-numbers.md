[Metadata]
title: Gas Station Numbers
date: 2018-02-03 23:31:54

[Tags]
difficulty: 3
categories: implementation
source: POJ

[Description]

Many gas stations use plastic digits on an illuminated sign to indicate prices. When there is an insufficient quantity of a particular digit, the attendant may substitute another one upside down. 
The digit "6" looks much like "9" upside down. The digits "0", "1" and "8" look like themselves. The digit "2" looks a bit like a "5" upside down (well, at least enough so that gas stations use it). 

Due to rapidly increasing prices, a certain gas station has used all of its available digits to display the current price. Fortunately, this shortage of digits need not prevent the attendant from raising prices. She can simply rearrange the digits, possibly reversing some of them as described above. 

Your job is to compute, given the current price of gas, the **next highest price** that can be displayed using exactly the same digits. 

The number of the digits could be as large as 1000, and there is no leading zeros. If the price cannot be raised, print `The price cannot be raised.`.

### Input / Output

```
Sample Input

652
767
777
.

Sample Output

655
776
The price cannot be raised.
```


### Source

[POJ2611 - Gas Station Numbers](http://poj.org/problem?id=2611) (with modifications)

[Solution]

For starters, we try to simplify this problem like we always do.

> Given the number, try to find the **next highest price** by just reordering the digits.

You may acquainted with this version because it's the classical "next permutation" problem.

For example, we've got the number '123', the next higher price should be '132'. Because '132' is one step greater than '123' by the lexicographical order.

Let's review the "next permutation" algorithm:

1. find the position of a digit `D` when every number to its right is in **descending order**
2. find the next larger digit `L` to the right of digit `D`
3. swap `D` and `L`
4. put the numbers on the right side of `L` in ascending order

Similarly, we can implement a updated version algorithm to the numbers in current problem. But there are some tricks:

1. as we can "flip" the numbers, so we have to take it into consideration when we are trying to find the "larger digit"
2. before we put the remain numbers on the right side in ascending order, we have to flip some numbers to make it as small as possible

Here is the code:

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <functional>
#include <numeric>
#include <set>
#include <map>
#include <cassert>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

const int INF = 0x3f3f3f3f;

map<char, char> gm;
map<char, char> lm;

string solve(string num) {
    int n = num.size();
    for (int i = n - 1; i >= 0; i--) {
        int ptr = -1;
        char mini = '~';
        bool flip = false;

        for (int j = i; j < n; j++) {
            char cur = num[j];
            char fur = gm.count(cur) == 0? cur: gm[cur];

            if (cur > num[i] && cur < mini) {
                mini = cur;
                ptr = j;
                flip = false;
            }

            if (fur > num[i] && fur < mini) {
                mini = fur;
                ptr = j;
                flip = true;
            }
        }

        if (ptr == -1) {
            continue;
        }

        swap(num[i], num[ptr]);
        if (flip) {
            num[i] = gm[num[i]];
        }

        for (int j = i + 1; j < n; j++) {
            num[j] = lm.count(num[j]) == 0? num[j]: lm[num[j]];
        }

        sort(num.begin() + i + 1, num.end());
        return num;
    }
    return "";
}


int main() {
    string num;

    gm['2'] = '5';
    gm['6'] = '9';
    gm['5'] = '2';
    gm['9'] = '6';

    lm['5'] = '2';
    lm['9'] = '6';

    assert(solve("2300") == "3002");
    assert(solve("199") == "616");
    assert(solve("12345") == "12354");

    while (input(num) && num != ".") {
        string res = solve(num);

        if (res.empty()) {
            puts("The price cannot be raised.");
        } else {
            print(res);
        }
    }

    return 0;
}
```