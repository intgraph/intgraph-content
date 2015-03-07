[Metadata]
title: Match the character sets
date:  2015-03-07 17:47:24 

[Tags]
difficulty: 2
categories: string, hash table
source: unknown

[Description]

This problem is about string matching. But for the string in this problem, the sequence of the characters is irrelevant. For instance, `aabbcc`, `bbaacc` and `abcabc` are actually the same sting. We call this kind of string as "the character sets".

Your missions are:

* find a way to match two character sets
* find a way to detect the same character sets form multiple sets

And the character sets will only contain lowercase letters `a-z`.

[Solution]

The intuitive and brute force way is to sort every character sets, and then the problem is transformed to the string matching problem.

However, this is not the effective solution. The time complexity of matching two sets is `O(sort:NlogN) + O(string matching:N) -> O(NlogN)`.

There is another way to match the sets. As the sets will only contains lowercase letters. It's easy to come up with the idea of the hash table. We use an array of size 26 to count the number of letter in the string. And then do the matching. For this method, the time complexity is `O(count:N) + O(matching:1) -> O(N)`.

How about matching multiple sets? Hash table, still. And we have to find a effective hash function to make this work.

The traditional way to generate the signiture of a string is something like this:

```python
def string_hash(s, MOD):
    g = 1
    res = 0
    for c in s:
        res = (res + g * (ord(c) - ord('a'))) % MOD
        g = (g * 26) % MOD
    return res
```

But for this problem, this method is not correct until we sort the character set. It's not acceptable because the time complexity of sorting. Then, we come up with this way:

```python
prime_list = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101]

def char_set_hash(s, MOD):
    res = 1
    for c in s:
        u = prime_list[ord(c) - ord('a')]
        res = (res * u) % MOD
    return res
```

It is the method which works well in this scenario, because the order of the string in irrelevent. And for every letters, the hash value is orthogonal. If we don't care about the range of the integer (or a long interger / long long interger), the hash value is distinct for every character sets.
