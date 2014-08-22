[Metadata]
title: Watering Flowers
date: 2014-08-22

[Tags]
categories: binary search, greedy, binary indexed tree
difficulty: 4
source: codeforces

[Description]
Little beaver plants N flowers in a row. The height of these flowers are [a1, a2 ... an]. 

Now you have a magic watering machine which can water W contiguous at the same time and these flowers will grow by one height in one day. But little beaver can only use the machine once a day. 

Your mission is to help beaver find the maximum possible final height of the smallest flower in M days.

### Input
The first line contains space-separated integers n, m and w (1 ¡Ü w ¡Ü n ¡Ü 10^5; 1 ¡Ü m ¡Ü 10^5). The second line contains space-separated integers a1,?a2,?...,?an (1 ¡Ü ai ¡Ü 10^9).

### Output
Print a single integer ¡ª the maximum final height of the smallest flower.

### Sample tests
```
>> input
6 2 3
2 2 2 2 1 1
<< output
2
```

```
>> input
2 5 1
5 8
<< output
9
```
[Solution]
Assuming the maximum possible minimum height of the flowers is K.

As the flowers stands in a line, so the optimal way to watering the leftmost flower ``fi`` which is smaller than K is to watering all the W flowers start from ``fi`` until the height of the ``fi`` equals to what we want. And then, we find another **leftmost flower smaller then K**, and do the previous steps again until the M day runs out.

As we discussed above, if we determine the value of K, we can tell that if the flowers will grow as our wish in M days.

So we enumerate the value of K in the binary search way, and we can find the maximum possible K.

One other thing to note that the value of W is ``[0, 10^5]``. If you add the height of the W contiguous  flowers one by one, the time complexity of the algorithm ``O(N * M * logK)``, you will get a TLE. So we use binary search tree to do that trick. 

A variant of binary search tree(BIT for short) -- SegBIT could add on segment in O(logN) time and get the value of one node in the array in O(logN). This structure will speed up your algorithm and pass the test eventually.

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

const int SIZE = 123456;

typedef long long llint;

int n, m, w;

inline int lowbit(int x){return x&(-x);}

struct SegBIT
{
    llint baum[SIZE];
    inline void init()
    {
        memset(baum,0,sizeof(baum));
    }
    void add(int x,int val)
    {
        while(x>0)
        {
            baum[x]+=val;
            x-=lowbit(x);
        }
    }
    void addseg(int a,int b,int val)
    {
        add(a-1,-val);
        add(b,val);
    }
    llint sum(int x)
    {
        llint res=0;
        while(x<SIZE)
        {
            res+=baum[x];
            x+=lowbit(x);
        }
        return res;
    }
};

SegBIT segbit;
int flowers[SIZE];

int solve(llint v)
{
    int step = m;
    SegBIT sb = segbit;
    for (int i = 1; i <= n; i++) {
        if (sb.sum(i) < v) {
            int diff = v - sb.sum(i);
            step -= diff;
            if (step < 0) {
                return 0;
            }
            sb.addseg(i, min(n, i + w - 1), diff);
        }
    }
    return 1;
}

int main()
{
    input(n >> m >> w);
    segbit.init();
    for (int i = 1; i <= n; i++) {
        scanf("%d", flowers + i);
        segbit.addseg(i, i, flowers[i]);
    }

    llint l = 0, r = 1LL << 30;
    while (l <= r) {
        llint mid = (l + r) >> 1;
        if (!solve(mid)) {
            r = mid - 1;
        } else {
            l = mid + 1;
        }
    }
    print(r);
    return 0;
}
```