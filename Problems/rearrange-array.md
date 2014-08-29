[Metadata]
title: Rearrange array in alternating positive & negative items
date: 2014-08-29 08:56:45 

[Tags]
difficulty: 2.5
categories: partition， implementation
source: geeksforgeeks, amazon

[Description]
Given an array of positive and negative numbers, arrange them in an alternate fashion such that every positive number is followed by negative and vice-versa maintaining the order of appearance.

Number of positive and negative numbers need not be equal. If there are more positive numbers they appear at the end of the array. If there are more negative numbers, they too appear in the end of the array.

Example:

```
Input:  arr[] = {1, 2, 3, -4, -1, 4}
Output: arr[] = {-4, 1, -1, 2, 3, 4}

Input:  arr[] = {-5, -2, 5, 2, 4, 7, 1, 8, 0, -8}
output: arr[] = {-5, 5, -2, 2, -8, 4, 7, 1, 8, 0} 
```

Please deal with it with O(n) time complexity and O(1) extra memory space.

[Solution]
This problem is quite easy, and almost as same as the ``partition`` operation in quick sort.

```cpp
// C++11 required
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cassert>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

const int SIZE = 1024;
vector<int> vec;

bool do_check()
{
    bool positive = false;
    for (auto i: vec) {
        if (i <= 0 && positive) {
            return false;
        }

        if (i > 0) {
            positive = true;
        }
    }
    return true;
}

int main()
{
    int T = SIZE;
    int n, a, b;
    vec.reserve(SIZE);
    while (T--) {
		input(n);
        vec.clear();
        for (int i = 0; i < n; i++) {
            scanf("%d", &a);
            vec.push_back(a);
        }
        a = 0; b = n - 1;
        while (true) {
            while (a < b && vec[a] <= 0) a++;
            while (a < b && vec[b] > 0)  b--;
            if (a < b) {
                swap(vec[a], vec[b]);
            } else {
                break;
            }
        }
		for (auto i: vec) {
			printf("%d ", i);
		}
		puts("");
        assert(do_check());
    }
    return 0;
}
```
