[Metadata]
title: Web Navigation
date: 2014-08-15 16:46:00

[Tags]
difficulty: 2
categories: simulation, implementation, no algorithm, STL
source: POJ, qunar

[Description]
Write a problem to simulate the web navigation, including "FORWARD", "BACK" and "VISIT". The behaviour is as same as a normal web browser.

In addition, our navigation is start from "http://www.acm.org/" (a porn website, maybe). Please show the URL of the current page after every command, print "Ignored" if the command leads to a error.

### Sample Input

Sample Input

```
BACK
VISIT http://www.baidu.com/
BACK
FORWARD
QUIT
```

### Sample Output

```
Ignored
http://www.baidu.com
http://www.acm.org/
http://www.baidu.com/
```

[Solution]
We can use an array to simulate our navigation, but an array is a static data structure, and may not capable for this problem because you may have a LONG LONG navigation history.

As a result, we have to use dynamic container to handle this problem. *Two stacks* is a good choice but it far from the best.

In my opinion, std::list is the right container for our navigation, and the code will be simple enough that I will not explain.

```cpp
// Result: wizmann  1028    Accepted    716K    32MS    G++ 1148B   2014-08-15 09:39:26
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <list>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

int main()
{
    string op1, op2;
    list<string> lst;
    lst.push_back("http://www.acm.org/");
    list<string>::iterator iter = lst.begin();
    
    while(input(op1) && op1 != "QUIT") {
        if (op1 == "VISIT") {
            input(op2);
            // using auto in C++11 will be much more convenient
            // and std::prev and std::next in C++11 will avoid the temp variables here
            list<string>::iterator now = iter, next = ++iter; 
            lst.erase(iter, lst.end());
            lst.push_back(op2);
            iter = --lst.end();
            print(op2);
        } else if (op1 == "BACK") {
            if (iter == lst.begin()) {
                print("Ignored");
            } else {
                --iter;
                print(*iter);
            }
        } else if (op1 == "FORWARD") {
            if (iter == --lst.end()) {
                print("Ignored");
            } else {
                ++iter;
                print(*iter);
            }
        }
    }
    return 0;
}
```
