[Metadata]
title: All Twice Excpet One
date: 2014-08-13 23:14:00

[Tags]
difficulty: 2
categories:  bit manipulation
source: unknown

[Description]
Given an array of integers, every element appears twice except for one. Find that single one.

[Solution]
This problem is the very basic one. So I just paste my code here. If you are confused with the xor operator， you might have to review the knowledge of **bit manipulation** carefully.

```python
class Solution:
    # @param A, a list of integer
    # @return an integer
    def singleNumber(self, A):
        return reduce(lambda x, y: x ^ y, A)
```

Time complexity: O(n)

