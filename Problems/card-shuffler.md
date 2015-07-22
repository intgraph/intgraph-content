[Metadata]
title: Card Shuffler
date: 2015-07-20 10:00:22

[Tags]
difficulty: 3.5
categories: implementation
source: google

[Description]

Given a card shuffler. Your mission is to find out after several times of shuffling, if there a possibility that the cards stay in the original permutation. If yes, return the minimal period of suffling. If not, return `-1`.

For example, we get the cards `[A, B, C, D, E]`. The shuffler is `[0, 1, 2, 3, 4]`. So the next permutation of the cards is still `[A, B, C, D, E]`. The minimal period of suffling is `1`.

Another example, the cards `[A, B, C, D, E]` with the shuffler `[4, 3, 2, 1, 0]`. The next permutation is `[E, D, C, B, A]`. And after two times of shuffling, the permutation is "rewind". So the answer for this is `2`.

[Solution]

If a shuffler could be "rewind", every elements could come back to its original position after several steps. We can find out that by a dfs-like algorithm. If the path could be a circle, e.g. `1 -> 2 -> 3 -> 1`, it's easy to know that the element with index `1` could be rewind. Additionally, the number `2` and number `3` could be rewined, either. We can call all the elements on the circle as "shuffle group".

There might be serveral "shuffle groups" in the suffler. And the shuffle period is the LCM(Lowest Common Multiple) of all the periods of sub-groups.

```cpp
class CardShuffler {
    typedef long long llint;
public:
    llint solve(const vector<int>& shuffle_array) {
        n = shuffle_array.size();
        visit = vector<int>(n, 0);

        vector<int> steps;
        for (int i = 0; i < n; i++) {
            if (visit[i]) {
                continue;
            }
            mp.clear();
            int step = dfs(i, 0, shuffle_array);
            // ASSERT(step != 0);
            if (step == -1) {
                return -1;
            }
            steps.push_back(step);
        }
        llint lcm_value = 1;
        for (auto step: steps) {
            // risk of integer overflow here.
            lcm_value = lcm(lcm_value, step);
        }
        return lcm_value;
    }
private:
    llint gcd(llint a, llint b) {
        if (a % b == 0) {
            return b;
        }
        return gcd(b, a % b);
    }
    llint lcm(llint a, llint b) {
        return a * b / gcd(a, b);
    }
    int dfs(int pos, int s, const vector<int>& shuffle_array) {
        visit[pos] = 1;
        if (mp.find(pos) != mp.end()) {
            return mp[pos] == 0? s: -1;
        }
        mp[pos] = s;
        return dfs(shuffle_array[pos], s + 1, shuffle_array);
    }
private:
    unordered_map<int, int> mp;
    vector<int> visit;
    int n;
};
```
