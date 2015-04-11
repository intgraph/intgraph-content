[Metadata]
title: Milking Cows - 2
date: 2015-04-10 22:48:01 

[Tags]
difficulty: 3
categories: greedy
source: codeforces

[Description]

There are n cows sitting in a row, numbered from 1 to n from left to right. Each cow is either facing to the left or facing to the right. When Iahub milks a cow, all the cows that see the current cow get scared and lose one unit of the quantity of milk that they can give. A cow facing left sees all the cows with lower indices than her index, and a cow facing right sees all the cows with higher indices than her index. A cow that got scared once can get scared again (and lose one more unit of milk). A cow that has been milked once cannot get scared and lose any more milk. You can assume that a cow never loses all the milk she can give (a cow gives an infinitely amount of milk).

The farmer wants to lose as little milk as possible. Print the minimum amount of milk that is lost.

The number of cows is n (1 ≤ n ≤ 200000) will show in the first line. The second line contains n integers a1, a2, ..., an, where ai is 0 if the cow number i is facing left, and 1 if it is facing right.

### Sample test(s)

```
>> input
4
0 0 1 0
<< output
1
>> input
5
1 0 1 0 1
<< output
3
```

[Solution]

I know you guys have got multiple ideas about this problem, and of course, some of these solutions are "seems correct" as you guys can't **prove the correctness** of your idea.

```
< < > <
```

The example case 1, for example. It seems that it's hard to decide to milk which cow at the very beginning. Maybe we can start from the rightmost cow which looking left, or the leftmost one which looking right. However, it may lead you to some kinds of pitfalls. And makes you spend several hours more to find the final answer.

This problem looks quite easy. It's not. (Maybe you can get the "Accepted" quite easily without your proof. But it can't really do the trick when you are having an interview.)

> There's a certain category of questions that fall into that sweet spot of "takes a tiny bit of thinking but not too much." 

So, before you make your final design, please make sure that your solution is reasonable, and make your best to prove its correctness.

I find an excellent [tutorial](http://codeforces.com/blog/entry/10476) for this problem.

For every two cows in that line. We get four scenarios.

1. \<left, left\>      
We can start from the rightmost one. And no milk will be losed.

2. \<left, right\>     
`<, >`. This two cows can't see each other. Arbitrary order will do.

3. \<right, right\>     
Similar to scenario 1, start from the leftmost one.

4. \<right, left\>      
`>, <`. This one is special, becase we can't make any strategy to avoid losing any milk. So, when we come across this kind of pair of cows. Add 1 to the final results.

And with some optimization tricks. We can find the final solution with O(n) time complexity and O(1) extra memory space.

```python
n = int(raw_input())
cows = map(int, raw_input().split())
cnt = 0
res = 0
for cow in cows:
    cnt += cow
    if not cow:
        res += cnt
print res
```
