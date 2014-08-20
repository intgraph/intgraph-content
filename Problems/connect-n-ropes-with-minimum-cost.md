[Metadata]
title: Connect n ropes with minimum cost
date: 2014-08-19 11:03:00

[Tags]
difficulty: 2.5
categories: priority-queue, huffman coding, heap
source: geeksforgeeks, POJ

[Description]
There are given n ropes of different lengths, we need to connect these ropes into one rope. The cost to connect two ropes is equal to sum of their lengths. We need to connect the ropes with minimum cost.

For example if we are given 4 ropes of lengths 4, 3, 2 and 6. We can connect the ropes in following ways.
1) First connect ropes of lengths 2 and 3. Now we have three ropes of lengths 4, 6 and 5.
2) Now connect ropes of lengths 4 and 5. Now we have two ropes of lengths 6 and 9.
3) Finally connect the two ropes and all ropes have connected.

Total cost for connecting all ropes is 5 + 9 + 15 = 29. This is the optimized cost for connecting ropes. Other ways of connecting ropes would always have same or more cost. For example, if we connect 4 and 6 first (we get three strings of 3, 2 and 10), then connect 10 and 3 (we get two strings of 13 and 2). Finally we connect 13 and 2. Total cost in this way is 10 + 13 + 15 = 38.

[Solution]
This problem is such a classical one that almost every programmer knows. But how to prove it is the truly correct answer?

However, the first thing first is always showing my code.

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

typedef long long llint;

template<typename T>
class MinHeap: public priority_queue<T, vector<int>, greater<T> > {};

int n;
MinHeap<llint> min_heap;


int main()
{
	int a;
	input(n);
	for(int i = 0; i < n; i++) {
		scanf("%d", &a);
		min_heap.push(a);
	}

	llint ans = 0;
	while (min_heap.size() >= 2) {
		llint a = min_heap.top();
		min_heap.pop();
		llint b = min_heap.top();
		min_heap.pop();
		min_heap.push(a + b);
		ans += a + b;
	}
	print(ans);
	return 0;
}
```

And now, let prove that solution is right. The solution is a variation of huffman encoding, and I suppose you know it well.

First of all, it's easy to us to know the "huffman tree" is a FULL BINARY TREE.

Secondly, I gonna prove that the minimal two nodes are both leaf nodes. 

Assuming there are two nodes which values are Ci and Cj. Ci is smaller and deeper than Cj. So, the sum of that tree is ``TREE = Li * Ci + Lj * Cj + OTHER_NODES``, if we swap Ci and Cj, ``TREE2 = Li * Cj + Lj * Ci + OTHER_NODES``. It's easy for us to see that ``TREE <= TREE2``. We can make a conclusion that the smaller node has to be a deeper node.

So, the **minimal** two nodes must be the leaf nodes.

At last, we combine the two minimal nodes as ONE whose value is equal to Ci + Cj. And the value of the whole tree will not change. So we make the Tree(n) problem into Tree(n - 1) problem. And the solution is right here.

