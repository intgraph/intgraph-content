[Metadata]
title: Arrow road
date: 2017-10-17 00:36:19

[Tags]
difficulty: 4
categories: array, binary search
source: Unknown

[Description]

Given a road with n grids. Each grid has an arrow in it, pointing to left or right. 

When you step on a grid with an arrow, the arrow will disappeard. And at the same time, you will move by the direction of the arrow, until you step on another arrow or you move out of the road.

![](http://wizmann-pic.qiniudn.com/17-10-16/88926922.jpg)

You can start from arbitrary grid, and here we want to know the maximum grids you can step on during the whole process until you move out of the road.

```
Input1     
<<>>><<     
Output1     
7

Input2
>><><<<
Output2
7
```

[Solution]
First problem of this problem is the given sample is too complicated to understand. Here is some simple ones:

```cpp
assert(solve(1, "<") == 1);
assert(solve(1, ">") == 1);
assert(solve(2, "><") == 2);
assert(solve(3, "><<") == 3);
assert(solve(4, ">><<") == 4);
assert(solve(4, "><><") == 4);
```

From the examples, we can find out that the total steps on the road depends on:

* the direction of the initial grid
* the number of "right arrow"(>) on the left of the initial grid 
* the number of "left arrow"(<) on the right of the initial grid

For example, if we start from the second grid of the road `>><<`, we can step on every grid of that road because the number of the left arrows and right arrows are the same. 

And for the road `>>><<`, we can also walk through all the grid when we start from the second or the third grid. The difference between the two choices is the final direction after walking through all the grids. So, for the road `<<>>><<`, we will take the fourth gird, and for the road `>>><<>>`, we will take the third.

According to the discussion above, we know we can predict the final status by counting the number of arrows on the left and right side of the initial position.

* If the grid we choose is '>'
    * if number of ">" on the left (mark as `l`) is greater than the number of "<" on the right (mark as `r`)
        * the final direction after the moving is to the left (of course)
        * we will step on at most `min(l, r)` right arrows on the left side of the initial grid
        * and another right arrow on the initial grid
        * we will step on at most `min(l, r) + 1` left arrows on the right side of the initial grid
    * otherwise
        * the final direction after the moving is to the right
        * we will step on at mose `min(l, r)` right arrows on the left side
        * and another right arrow on the initial grid
        * we will step on at most `min(l, r) + 1` left arrows on the right side
* If the gird we choose is '<', it's obviously symmetrical to the previous condition
    * and we will skip that

In addition, if there is no enough arrows on the left or right, we will take the boundaries (the position `0` and `n - 1`) as the final position.
    

By the prefix sum algorithm, we can get the number of the arrows (both left and right) with `O(n)` preprocessing and `O(1)` for each query. And using binary search algorithm to find the specific position with specific number of arrows.

The code will be like this:


```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>
#include <cassert>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

int solve(int n, const string& s) {    
    // begin preprocessing
    vector<int> left(n, 0);
    vector<int> right(n, 0);

    for (int i = 0; i < n; i++) {
        left[i] += i - 1 >= 0? left[i - 1]: 0;
        left[i] += s[i] == '>';
    }

    for (int i = n - 1; i >= 0; i--) {
        right[i] += i + 1 < n? right[i + 1]: 0;
        right[i] += s[i] == '<';
    }
    // end preprocessing

    int ans = 0;
    for (int i = 0; i < n; i++) {
        int l = left[i];
        int r = right[i];
        int ld = -1;
        int rd = -1;
        
        // begin calculation the number of arrows
        if (s[i] == '>') {
            if (l > r) {
                ld = min(l, r) + 1;
                rd = min(l, r) + 1;
            } else {
                ld = min(l, r) + 1;
                rd = min(l, r);
            }
        } else {
            if (r > l) {
                rd = min(l, r) + 1;
                ld = min(l, r) + 1;
            } else {
                ld = min(l, r);
                rd = min(l, r) + 1;
            }
        }
        // end calculation the number of arrows
        
        // begin get the leftmost and right most position
        int lp = lower_bound(
            left.begin(), 
            left.begin() + i + 1, 
            l - ld + 1) - left.begin();
        int rp = upper_bound(
            right.begin() + i, 
            right.end(), 
            r - rd + 1, 
            greater<int>()) - right.begin() - 1;
        // end get the leftmost and right most position
        
        lp = max(0, lp);
        rp = min(n - 1, rp);
        
        ans = max(ans, rp - lp + 1);
    }
    return ans;
}

// brute force method for testing
int brute_force(int n, const string& s) {
    int ans = 0;
    for (int i = 0; i < n; i++) {
        string ss = s;
        int cnt = 0;
        int p = i;
        while (p >= 0 && p < n) {
            int dir = ss[p] == '>'? 1: -1;
            ss[p] = 'x';
            cnt++;
            while (p >= 0 && p < n && ss[p] == 'x') {
                p += dir;
            }
        }
        ans = max(ans, cnt);
    }
    return ans;    
}

int main() {
    int n;
    string s;
    
    assert(solve(4, "<><<") == 4);
    assert(solve(4, "<><>") == 3);
    assert(solve(4, "><<>") == 4);
    
    assert(solve(1, "<") == 1);
    assert(solve(1, ">") == 1);
    assert(solve(2, "><") == 2);
    assert(solve(3, "><<") == 3);
    assert(solve(4, ">><<") == 4);
    assert(solve(4, "><><") == 4);
    
    assert(solve(4, "<<<<") == 4);
    assert(solve(4, ">>><") == 4);
    assert(solve(6, ">>>>><") == 6);
    assert(solve(7, "<<><<>>") == 5);
    assert(solve(3, "<>>") == 2);
    assert(solve(2, "<>") == 1);
    assert(solve(4, "<<>>") == 2);
    assert(solve(5, "<<>>>") == 3);
    assert(solve(7, "<<>>><<") == 7);
    assert(solve(7, ">><><<<") == 7);
    
    
    const int N = 10;
    for (int i = 0; i < (1 << N); i++) {
        string s;
        for (int j = 0; j < N; j++) {
            if (i & (1 << j)) {
                s += '>';
            } else {
                s += '<';
            }
        }
        assert(solve(s.size(), s) == brute_force(s.size(), s));
    }
    
    print("done");
    
    input(n);
    input(s);

    print(solve(n, s));

    return 0;
}
```