[Metadata]
title: One Edit Distance++
date: 2015-07-22 09:49:39

[Tags]
difficulty: 4
categories: implementation
source: Facebook

[Description]

From the [previous problem](http://intgraph.wizmann.tk/Problems/one-edit-distance.html), we have learned something about the "one-or-zero edit distance". And here is an advanced problem.

We have two strings `A` and `B`, but we can only access the two strings by the **iterators**.

```cpp
class StrIterator {
	bool has_next();
	bool is_null();
	int next();
	char get();
};
```

That is, we can only be aware of the current character, and if there a following character. We have no idea about the length of the string. As a result, this problem is harder than the previous one.

Try your best to solve it. :)

[Solution]

The intuitive thought for the problem is DFS. We would search for all possibilities if there is a dismatch between `A` and `B`.

Because there is only one or zero dismatch between the strings, the time complexity is `O(n)`. However, we should do better than that.

Before we come up our solution, the test cases should come first.


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

For case1, we just iterate through the strings, then reach the end, and find no dismatch.

But for the rest cases, it's hard to detect because there are multiple choices for us to manipulate the iterators.

For exmaple, `abcxxx` and `abdxxx`. We find the dismatch at position 3, between the character `c` and `d`. Because we have no knowledge of the length of the strings, and it's might be OK to delete `c` or `d`, or just ignore this dismatch.

That's true. Since our lack of knowledge of the two strings, we have to consider every possibilities. Luckily, if we just look forward for one single character of both strings, all the possibilities could be settled.

```cpp
/*
class StrIterator {
	bool has_next();
	bool is_null();
};
*/

class OneEditDistance {
	bool one_or_zero_edit_distance(
			StrIterator iter_A,
			StrIterator iter_B) {
		while (!iter_A.is_null() && !iter_B.is_null()) {
			if (*iter_A != *iter_B) {
				break;
			}
			iter_A++;
			iter_B++;
		}
		if (iter_A.is_null()) {
			// abc  vs abcc
			//    ^       ^
			//  iter_A  iter_B
			// ---
			// abc   vs abc
			//    ^        ^
			//  iter_A   iter_B
			return iter_B.has_next();
		}
		if (iter_B.is_null()) {
			return iter_A.has_next();
		}

		auto iter_AA = iter_A;
		++iter_AA;

		auto iter_BB = iter_B;
		++iter_BB;

		int dismatch[3] = {0};

		while (!iter_AA.is_null() && !iter_BB.is_null()) {
			if (*iter_AA != *iter_BB) {
				dismatch[0]++;
			}
			if (*iter_A != *iter_BB) {
				dismatch[1]++;
			}
			if (*iter_AA != *iter_B) {
				dismatch[2]++;
			}
		}

		bool ans = false;
		for (int i = 0; i < 3; i++) {
			ans |= dismatch[i] == 0;
		}

		return ans && iter_AA.is_null() && iter_BB.is_null();
	}
};

```
