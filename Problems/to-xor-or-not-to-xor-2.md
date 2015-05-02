[Metadata]
title: To xor or not to xor - 2
date: 2015-05-02 12:56:44 

[Tags]
categories: xor, trie
difficulty: 3
source: unknown

[Description]

The sequence of non-negative integers A1, A2, ..., AN is given. You are to find a sebset of the sequence, such that the xor of the subset has a maximum value.

**The length of the sequence N could be up to 40.**

**The max value of the sequences is up to (2^21 - 1)**

Sample test(s)

```
Input
3       <- The length of the sequence
11 9 5  <- The numbers in the sequence

Output
14      <- The max xor value for a subset of the sequcence
```

[Solution]

Of course, you can solve this problem using the way like the problem "To xor or not to xor". But it's a too heavy method for this one. Because the length of the sequence could only up to 40. See, it is possible to seperate the sequence into two pieces, and merge them to find the final answer. But how?

Greedy. Assuming we get a batch of xor sum of the subset of the sequence. For every xor sum in the other batch, the way to make a xor sum as large is possible is to find if there a number which is close to the **BITWISE NOT** of the current number.

For example, we got the number 5, which is "101" in binary. And for every three bit numbers, if we get "010", then we can form a largest xor sum. If we don't got one, the number "011" is not bad. And then "000", "001" and so on.

Now it's clear that we will use a greedy algorithm to find the number which can make the largest sum. And using a "trie" can make the time complexity to `O(MAX LENGTH OF BIT)` for every searching.

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

const int BIT_LENGTH = 20;
const int TRIE_SIZE = (1 << BIT_LENGTH) * 2;
const int BATCH_SIZE = 20;
const int ROOT = 0;

inline int LEFT(int x) { return 2 * x + 1; }
inline int RIGHT(int x) { return 2 * x + 2; }

int trie[TRIE_SIZE][2];

int n;
int ans;
vector<int> nums;

vector<int> batch_left;
vector<int> batch_right;

void do_batch(vector<int>& batch, int st, int end) {
    int num = end - st;
    if (num <= 0) {
        return;
    }
    for (int i = 0; i < (1 << num); i++) {
        int sum = 0;
        for (int j = 0; j < num; j++) {
            if (i & (1 << j)) {
                sum ^= nums[st + j];
            }
        }
        batch.push_back(sum);
        ans = max(ans, sum);
    }
}

void trie_insert(int p, int num, int b) {
    int u = (num & (1 << b))? 1: 0;
    if (trie[p][u] == -1) {
        trie[p][u] = 1;
    }
    if (b - 1 < 0) {
        return;
    }
    trie_insert(
        u == 0? LEFT(p): RIGHT(p),
        num,
        b - 1
    );
}

void make_trie() {
    for (auto& i: batch_left) {
        trie_insert(ROOT, i, BIT_LENGTH);
    }
}

int do_search(int p, int num, int b) {
    int u = (num & (1 << b))? 1: 0;
    if (b < 0) {
        return 0;
    }
    if (trie[p][u ^ 1] != -1) {
        return (1 << b) | do_search(
                (u ^ 1) == 0? LEFT(p): RIGHT(p), num, b - 1);
    } else {
        return do_search(
                u == 0? LEFT(p): RIGHT(p), num, b - 1);
    }
    return 0;
}

void search() {
    for (auto& i: batch_right) {
        ans = max(
            ans,
            do_search(ROOT, i, BIT_LENGTH)
        );
    }
}

int main() {
    freopen("input.txt", "r", stdin);
    int a;
    while (input(n)) {
        ans = 0;

        nums.clear();
        batch_left.clear();
        batch_right.clear();

        memset(trie, -1, sizeof(trie));

        for (int i = 0; i < n; i++) {
            input(a);
            nums.push_back(a);
        }
        
        do_batch(batch_left, 0, min(n, BATCH_SIZE));
        do_batch(batch_right, min(n, BATCH_SIZE), n);

        make_trie();
        search();

        print(ans);
    }
    return 0;
}
```
