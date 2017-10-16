[Metadata]
title: Separate the Same Characters I
date: 2017-09-09 22:10:23

[Tags]
difficulty: 2.5
categories: brute force
source: hihoCoder

[Description]

Given a string contains only characters 'a' to 'z'. Please rearrange the string to make that any two same characters are not adjacent. If there are multiple answers, the output should be the one with minimal lexicographical order. If there is no answer, the output is "INVALID".

For example, the given string is "aaabc", and the answer could be "abaca".

> [hihoCoder - 1327](http://hihocoder.com/problemset/problem/1327)

[Solution]

It seems hard to find out a way to make a valid result string directly. But on the other hand, the difficulty of find out whether there is a valid answer is not as much high.

As we can see, if the number of any character is more than the half of the whole string, there's definitely no possible answer for any rearrangements. And if we know the numbers of characters in the string (or the substring), we can check the existance of the answer with this simple math:

```python
    if any count_of_chars(mystr) > (len(mystr) + 1) / 2:
        return False
    else:
        return True
```

So the problem will be solved with these steps:

1. Find a character with minimal possible lexicographical order in our "character bag", and add it to the result string
2. See if there is a possible solution with the rest of characters    
    2.1 if yes, go back to step 1       
    2.2 if no, put the character back to our "character bag", then go back to step 1

If we can't find any possible way, then print "INVALID" at the end of the procudure.

And of course, in our code, there are some ways for optimization. But the main idea is simple and straight-forward, if you are "brave" enough to come up with this brute-force algorithm.

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <map>
#include <queue>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

template<typename T>
class FuzzyMap {
public:
    FuzzyMap() {
        // pass
    }
    
    template<typename iterT>
    FuzzyMap(iterT st, iterT end) {
        for (auto iter = st; iter != end; ++iter) {
            add_value(*iter, +1);
        }
    }

    void set_value(const T& t, int value) {
        mp[t] = value;
        pq.push({value, t});
    }
    
    void add_value(const T& t, int delta) {
        mp[t] += delta;
        pq.push({mp[t], t});
    }
    
    int get_value(const T& t) {
        return mp[t];
    }
    
    int get_max(T& t, int& value) {
        while (!pq.empty()) {
            const auto& cur = pq.top();

            const int& cnt = cur.first;
            const T& item = cur.second;
            
            if (mp[item] != cnt) {
                pq.pop();
            } else {
                t = item;
                value = cnt;
                return 0;
            }
        }
        return -1;
    }
private:
    map<T, int> mp;
    priority_queue<pair<int, T> > pq;
};

FuzzyMap<char> mp;

bool check(char c, int l, int n) {
    mp.add_value(c, -1);
    
    char tmp = '\0';
    int maxi = 0;
    mp.get_max(tmp, maxi);
    
    mp.add_value(c, +1);
    
    return maxi * 2 <= n - l + 1;
}

string solve(const string& s) {
    string res = "";
    
    mp = FuzzyMap<char>(s.begin(), s.end());
    
    int n = s.size();
    char pre = '\0';
    for (int i = 0; i < n; i++) {
        bool flag = false;
        for (char c = 'a'; c <= 'z'; ++c) {
            if (c == pre) {
                continue;
            }
            
            if (mp.get_value(c) == 0) {
                continue;
            }
            
            if (check(c, i + 1, n)) {
                res += c;
                mp.add_value(c, -1);
                flag = true;
                pre = c;
                break;
            }
        }
        // print(res);
        if (!flag) {
            return "INVALID";
        }
    }
    return res;
}

int main() {
    string s;
    input(s);
    
    print(solve(s));
    
    return 0;
}
```