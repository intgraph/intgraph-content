[Metadata]
title: Bad Hair Day
date: 2014-08-14 11:01:00

[Tags]
difficulty: 3
categories: stack, monotone, thinking in reverse
source: POJ

[Description]
Given an array of size N (1 <= N <= 80000), represent a queue of cows with different height(1 <= height <= 10^9).

Therefore, cow i can see the tops of the heads of cows in front of her (namely cows i+1, i+2, and so on), for as long as these cows are strictly shorter than cow i.

For example,

```
        =
=       =
=   -   =         Cows facing right -->
=   =   =
= - = = =
= = = = = =
1 2 3 4 5 6 
```

* Cow#1 can see the hairstyle of cows #2, 3, 4
* Cow#2 can see no cow's hairstyle
* Cow#3 can see the hairstyle of cow #4
* Cow#4 can see no cow's hairstyle
* Cow#5 can see the hairstyle of cow 6
* Cow#6 can see no cows at all!

### Sample Input

6
10 3 7 4 12 2

### Sample Output

5

Link: http://poj.org/problem?id=3250

[Solution]

*Thinkin' in Rerverse*

"How many cows can be seen by a specific cow?" => "How many cows are able to see a specific cow?"

A cow can be seen by some other cows only if these cows is on the left side of the chosen one. And these cows must in decreasing order (may not be adjacent).

As a result, we keep a ** monotone ** stack to keep track on the DECREASING cows line which is at the left side and taller than this one.

```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <stack>
#include <iostream>
#include <algorithm>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

typedef long long llint;

const int SIZE = 88888;

int main()
{
	int n;
	llint cow, ans = 0;
	stack<llint> st;
	input(n);
	for (int i = 0; i < n; i++) {
		input(cow);
		while (!st.empty() && st.top() <= cow) {
			st.pop();
		}
		ans += st.size();
		st.push(cow);
	}
	print(ans);
	return 0;
}
```