[Metadata]
title: The pairs of separating circles
date: 2015-07-04 17:05:53 

[Tags]
difficulty: 3
categories: sorting
source: 51nod

[Description]

Given N circles, all the centres of the circles are on the x-axis. 

Now we have the positions and the radiuses of these circles. Please find out how many pairs of circles are separating.

![two separating circles][1]

Sample Input & Output
```
>> Input
4
1 1
2 1
3 2
4 1
<< Output
1
```
  [1]: http://wizmann-pic.qiniudn.com/a30dcb0060c2bb2d55bf6031c4c2254b
  
[Solution]

This problem is about circles. However, if the centres of the circles are on the same line, it turns into a problem of segemnts.

The key to such segments-on-a-single-line problem is sorting. And, we have to choose the key of sorting, the beginning of the segment or the end of that. In this problem, we take both.

If the beginning point of a segment (it's OK if you call it "circle") is on the right side of a end point of a segment. Obviously, they are a pair of seperating segments (a.k.a, seperating circles).

So, we sort the beginning points and end points. Iterating the sorted end points and find out how many segments are on the right side of the current one without intersections.

```python
n = int(raw_input())
lines = []

for i in xrange(n):
    p, r = map(int, raw_input().split())
    lines.append((p - r, p + r))
end_points   = sorted(map(lambda x: x[1], lines))
start_points = sorted(map(lambda x: x[0], lines))

ptr = 0
ans = 0
for i in xrange(n):
    e = end_points[i]
    while ptr < n and start_points[ptr] <= e:
        ptr += 1
    ans += (n - ptr)
print ans
```
