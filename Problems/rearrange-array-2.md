[Metadata]
title: Rearrange array in alternating positive & negative items - 2
date: 2014-09-10 09:42:49 

[Tags]
difficulty: 3
categories: implementation
source: geeksforgeeks

[Description]
Given an array of positive and negative numbers, arrange them such that the negative numbers are on the left side of the positive ones. In addition, the relative position of the numbers should not changed.

```
INPUT: 
-1 1 3 -2 2

ONLY ANSWER:
-1 -2 1 3 2
```

An O(n) time and O(1) extra space solution is perfect.

[Solution]
There are many discussion about this (or this kind) problem on the internet. But seldom of them have come up a real solution with O(n) time and O(1) space. And I consider that there is no **REAL** O(n) & O(1) solution, but there are some tricks to reduce the complexity.

### An O(n ** 2) & O(1) solution

```cpp
void bf_solve(vector<int>& vec)
{
    int n = vec.size();
    for (int i = 0; i < n; i++) {
        if (vec[i] < 0) {
            int p = i;
            while (p - 1 >= 0 && vec[p - 1] >= 0) {
                swap(vec[p], vec[p - 1]);
                p--;
            }
        }
    }
}
```

An intuitive and brute force solution.

### An O(n * logn) * O(1) solution which is NOT A REAL one

```cpp
int cmp(int a, int b)
{
    return abs(a % 10000) < abs(b % 10000);
}

void tricky_solve(vector<int>& vec)
{
    int n = vec.size();
    for (int i = 0; i < n; i++) {
        if (vec[i] < 0) {
            vec[i] = vec[i] * 10000 - (i + 1);
        } else {
            vec[i] = vec[i] * 10000 + (i + 1) * 100;
        }
    }

    sort(vec.begin(), vec.end(), cmp);
    for (int i = 0; i < n; i++) {
        vec[i] /= 10000;
    }
}
```

Tricky, huh~

### An O(n) & O(1) solution also NOT A REAL one

Like the solution above, we can storage the final position of the numbers at the last digits of the number. And we move them to the right place with O(n) time complexity and O(1) time.

And the code will not paste here because I don't like the tricky things. :)

The limitation of these tricky solution is that we can ganrantee that the numbers are have enough space to store the extra information. But in some conditions, it will work perfectly.
