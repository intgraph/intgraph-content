[Metadata]
title: Nested List Reversed Weighted Sum
date: 2015-03-07 23:04:36 

[Tags]
categories: dfs
difficulty: 1.5
source: linkedin

[Description]

Compute the reverse depth sum of a nested list meaning the reverse depth of
each node (ie, 1 for leafs, 2 for parents of leafs, 3 for parents of parents
of leafs, etc.) times the value of that node.

For example, the sum of the nested list: `[[1,2], 1, [2, [2,1]]]` is 13.

![Alt text](http://wizmann-pic.qiniudn.com/9302b7a5f83bfcc27be8e33310950399)

[Solution]

For the nested list, DFS is always the instant way to solve the problem.

```python
nested_list = [[1,2], 1, [2, [2,1]]]

def revSum(nlist):
    (depth, res) = dfs(nlist)
    return res

def dfs(nlist):
    nlist_sum = 0
    s = 0
    max_lv = 1
    for item in nlist:
        if isinstance(item, list):
            (depth, res) = dfs(item)
            nlist_sum += res
            max_lv = max(max_lv, depth + 1)
        else:
            s += item
    nlist_sum += s * max_lv
    return (max_lv, nlist_sum)

print revSum(nested_list)
```
