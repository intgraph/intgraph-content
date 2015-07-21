[Metadata]
title: One Edit Distance
date: 2015-07-22 01:22:01

[Tags]
difficulty: 3
categories: implementation
source: Leetcode

[Description]

Given two strings S and T, determine if they are both one edit distance apart.

Edit Distance:
minimum number of modifications to make a=b

1. change an int in a
2. insert an int to a
3. remove an int from a

For example,

```
OneEditDistance("abc", "abc") == True;
OneEditDistance("ab",  "abc") == True;
OneEditDistance("abc", "abd") == True;
```

[Solution]

Let's make write some test case first.

```
Case1:
abcxxx
abcxxx

Case2:
abcxxx
abdxxx

Case3:
abxxx
abcxxx

Case4:
abcxxx
abxxx
```

All these cases are all "zero or one edit distance apart". And we can find out some hints for the solution.

We can make a conclusion here that:

1. If length of string A and string B are the same, we can only modify the string for only once
2. If length of string A is a character longer than B, we should delete one character of string A to make them the same
3. If length of string B is a character longer than A, it's the same scenario as the previous one.
4. Otherwise, string A and string B has no "one-or-zero edit distance".

```python
def deal_len_delta_0(A, B):
	cnt = 0
	for i in xrange(len(A)):
		cnt += 1 if A[i] != B[i] else 0
	return cnt <= 1

def deal_len_delta_1(A, B):
	# assert len(A) == len(B) + 1
	pa = 0
	pb = 0
	cnt = 0
	while pa < len(A) and pb < len(B):
		if A[pa] != B[pb]:
			cnt += 1
			pa += 1
		else:
			pa += 1
			pb += 1

	a = (cnt == 1 and pa == len(A))
	b = (cnt == 0 and pb == len(B))
	return a or b


def solve(A, B):
	lenA = len(A)
	lenB = len(B)

	if lenA == lenB:
		print 'YES' if deal_len_delta_0(A, B) else 'NO'
	elif lenA == lenB + 1:
		print 'YES' if deal_len_delta_1(A, B) else 'NO'
	elif lenB == lenA + 1:
		print 'YES' if deal_len_delta_1(B, A) else 'NO'
	else:
		print 'NO'


if __name__ == '__main__':
	solve('abcxxx', 'abcxxx')
	solve('abcxxx', 'abdxxx')
	solve('abxxx',  'abcxxx')
	solve('abcxxx', 'abxxx')
	solve('xxx',    'yyy')

```
