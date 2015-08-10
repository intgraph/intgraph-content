[Metadata]
title: Palindrome Number
date: 2015-08-10 22:16:10

[Tags]
difficulty: 3
categories: implementation
source: google

[Description]

Given a number `x`, please find the minimal number `y` that `y > x` and `str(y)` is a palindrome.

For example

```
0 -> 1
11 -> 22
121 -> 131
233 -> 242
99 -> 101
```

[Solution]

The only key to this problem is to write the test case which covers every corner case of the problem.

Some crucial points:

1. if the given number is less than 10, simply add one to it.
2. if the given number only contains '9', the answer should be `1000..01`.
3. if the length of the number is odd, be ware of these cases : "190", "191", "192"

After all, the only thing left is to write down the code and be careful for all potential errors.

```python
def solve(num):
    if not filter(lambda x: x != '9', str(num)):
        return num + 2
    ans = []
    l = len(str(num))
    if l == 1:
        return num + 1
    if l % 2 == 0:
        left = int(str(num)[:l/2])
        ans.append(str(left) + str(left)[::-1])
        ans.append(str(left + 1) + str(left + 1)[::-1])
    else:
        left = int(str(num)[:l/2])
        mid  = int(str(num)[l/2])
        ans.append(str(left) + str(mid) + str(left)[::-1])
        ans.append(str(left) + str(mid + 1) + str(left)[::-1])
        ans.append(str(left + 1) + '0' + str(left + 1)[::-1])

    res = 0xdeadbeef
    for item in ans:
        if int(item) > num and item == item[::-1]:
            res = min(res, int(item))
    return res

def test(num):
    while True:
        num += 1
        if str(num) == str(num)[::-1]:
            return num

if __name__ == '__main__':
    import random
    for i in xrange(10000):
        x = random.randint(0, 0xcafebabe)
        if solve(x) != test(x):
            print x, solve(x), test(x)
            break
```
