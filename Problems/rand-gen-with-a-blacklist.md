[Metadata]
title: Random Generator with a Blacklist
date: 2015-05-17 00:40:38 

[Tags]
difficulty: 4
categories: binary search, hash, thinking in reverse
source: Google

[Description]

Your mission is to design a random generator with a given blacklist. That is, to generate random positive numbers with equal possibilities except the number on the blacklist.

[Solution]

### Scenario 1

> The range of the random number is small.

If the range of the random number is small, we can make a 1-to-1 mapping of the numbers which don't on the blacklist to make a consecutive sequence.

```python
class RandWithBlack(object):
    def __init__(self, range_, blacklist):
        cnt = 0
        self.d = {}
        for i in xrange(1, range_ + 1):
            if i in blacklist:
                continue
            self.d[i] = cnt
            cnt += 1
        self.size = len(self.d)
    
    def rand(self):
        r = random.randint(0, self.size - 1)
        return self.d[r]
    
```

The time complexity is `O(1)` for every request of a random number. But it has its limitations.

### Scenario 2

> The range of the random number is large and mutable.

If the range is large, it's impossible to mapping the numbers because the limitation of memory. And then, we should turn to the blacklist to find a solution.

![Alt text](http://wizmann-pic.qiniudn.com/5eef334236bd334e9317923839d115f4)

As the chart above, the key for us to solve the problem is to make a mapping about the number and the number except blacklist.

And it's not for us to see, if we've got a number k, and the number of numbers in blacklist which is smaller than k. Then we can finally make a map.

```cpp
class RandWithBlack {
public:
    RandWithBlack(const vector<int> blacklist): _blacklist(blacklist) {
        sort(_blacklist.begin(), _blacklist.end());
    }
    
    // @brief   get a random number except the ones in the blacklist
    // @params n    the range of the random number is [0, n], inclusively
    int rand(int n) {
        int l = 0;
        int r = n;
        
        int blacked = blacked_num(n);
        
        if (n - blacked == 0) {
            return -1;
        }
        
        int rand = random() % (n - blacked);
        while (l <= r) {
            int m = (l + r) >> 1;
            int mm = blacked_num(m);
            
            if (m - mm > rand) {
                r = m - 1;
            } else {
                l = m + 1;
            }
        }
        return l;
    }
private:
    int blacked_num(int v) {
        int res = distance(
            _blacklist.begin(),
            upper_bound(
                _blacklist.begin(),
                _blacklist.end(),
                v
            )
        );
        return res - 1;
    }
private:
    vector<int> _blacklist;
};
```

### Scenario 3

> The range of the random number is large but immuatble.

In this scenario, the solution is quite similar to the scenario 1. 

```python
class RandWithBlack(object):
    def __init__(self, range_, blacklist):
        cnt = 0
        self.d = {}
        range__ = range_ - len(blacklist)
        
        a = filter(lambda x: x <= range__, blacklist)
        b = filter(lambda x: x not in blacklist, 
                range(range__ + 1, range_ + 1))
            
        self.d = dict(zip(a, b))
        
        self.range_ = range__

    def rand(self):
        r = random.randint(0, self.range_)
        if r in self.d:
            return self.d[r]
        return r
```
