[Metadata]
title: Max Suffix Match
date: 2015-02-20 22:30:36 

[Tags]
difficulty: 4
categories: kmp, string
source: google

[Description]

A suffix (also sometimes called a postfix or ending) is an affix which is placed after the stem of a word.

For example, the suffixes of the world `banana` are: "a, na, ana, ...".

We define **suffix match** as the substring of a string which is not a suffix, but equals to one of the suffixes.

For example, one of the **suffix match** of the string `banana` is: `ana`.

```
banana
   ^
   suffix -> ana
substring -> ana
 v  
banana
```

[Solution]

Accroding to "suffix match", it's not hard to associate the "prefix match". And "prefix match" is the basic concept of the **KMP algorithm**. If we can get the "prefix match" of a string, the "suffix match" is the "prefix match" of the reversed string.

The algorith for KMP is like here below. The prefix match of the position `i` is `str[i - next[i]:i]`.

```cpp
void get_next(const string& needle) {
    next[0] = -1;
    int len = needle.size();
    int i = 0;
    int j = -1;

    while (i < len) {
        while (j >= 0 && needle[i] != needle[j]) {
            j = next[j];
        }
        i++;
        j++;
        next[i] = j;
    }
}
```

For example, the `next` array of the word `banana` is: `[-1, 0, 0, 0, 0, 0, 0]`, because there is no prefix match. And for the word `ananab`, the reversed of `banana`, the `next` array is `[-1, 0, 0, 1, 2, 3, 0]`.

And we can get all the prefix match like this:

```
def get_prefix_match(S):
    nexts = get_next(S)
    print S
    print nexts
    for i, u in enumerate(nexts):
        if u <= 0:
            continue
        v = u
        print '>>', S[:u], "\t[0...%d]" % u, S[i - u: i], "\t[%d...%d]" % (i - u, i)
```

And the suffix match is the prefix match of the reversed string.

```
def get_next(needle):
    l = len(needle)
    nexts = [0 for i in xrange(l + 1)]
    nexts[0] = -1
    i, j = 0, -1

    while i < l:
        while j >= 0 and needle[i] != needle[j]:
            j = nexts[j]
        i += 1
        j += 1
        nexts[i] = j
    return nexts

def get_prefix_match(S):
    nexts = get_next(S)
    print S
    print nexts
    for i, u in enumerate(nexts):
        if u <= 0:
            continue
        v = u
        print '>>', S[:u], "\t[0...%d]" % u, S[i - u: i], "\t[%d...%d]" % (i - u, i)

def get_suffix_match(S):
    return get_prefix_match(S[::-1])

if __name__ == '__main__':
    get_suffix_match('banana')
```

And the result is like this:

```
> python kmp.py
ananab
[-1, 0, 0, 1, 2, 3, 0]
>> a    [0...1] a       [2...3]
>> an   [0...2] an      [2...4]
>> ana  [0...3] ana     [2...5]
```


