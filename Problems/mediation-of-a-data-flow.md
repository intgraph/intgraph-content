[Metadata]
title: mediation of a data flow
date: 2014-09-13 10:21:31 

[Tags]
categories: priority-queue
difficutly: 3
source: unknown

[Description]
Given an array ``A[0...n-1]``, your task to find out the every mediation of ``A[0..0]`` to ``A[0...n-1]``.

[Solution]
The simple way to find out the mediation is to use the function ``nth_element``, which takes O(n) time and no extra memory space.

But our mission is to find out **every** mediation, and ``nth_element`` not works anymore.

Assuming we have find out the mediation ``M`` of the array ``A[0...k]``. And the array is sorted, it can be represent like this, ``sortedA[0...m-1] ++ sortedA[m] ++ sortedA[m+1...n-1]``. And if we add one number ``T`` into that array, to the left side or the right side, depends on T is less or greater than M. It's easy to find out that we have just to move the mediation pointer to the left or right by one step.

As a result, the total time complexity of our solution is ``O(nlogn)`` and needs extra ``O(n)`` memory space. Every operation to find out the mediation of the data flow is ``O(1)``.

```cpp
// random data tested
#include <cstdio>
#include <iostream>
#include <queue>
#include <vector>
#include <limits>

using namespace std;

#define print(x) cout << x << endl
#define input(x) cin >> x

template <typename T>
class MinHeap: public priority_queue<T, vector<T>, greater<T> >{};

template <typename T>
class MaxHeap: public priority_queue<T, vector<T>, less<T> >{};

class Mediation {
public:
    void push(int x);
    double mediation(); // the mediation may be a double number
    void clear();
    size_t size() { return _size; }
private:
    size_t _size;
    MinHeap<int> _min_heap;
    MaxHeap<int> _max_heap;
};

void Mediation::clear() {
    _min_heap = MinHeap<int>();
    _max_heap = MaxHeap<int>();
    _size = 0;
}

void Mediation::push(int x) {
    if (_max_heap.empty() || x <= _max_heap.top()) {
        _max_heap.push(x);
    } else {
        _min_heap.push(x);
    }
    _size++;
}

double Mediation::mediation() {
    if (_max_heap.empty()) {
        return std::numeric_limits<double>::quiet_NaN();
    }
    size_t u = (_size + 1) / 2;
    while (_max_heap.size() < u) {
        _max_heap.push(_min_heap.top());
        _min_heap.pop();
    }
    while (_max_heap.size() > u) {
        _min_heap.push(_max_heap.top());
        _max_heap.pop();
    }
    if (_size & 1) {
        return _max_heap.top();
    } else {
        return (_max_heap.top() + _min_heap.top()) / 2.;
    }
}


int main()
{
    int a, n;
    Mediation med;
    while (input(n)) {
        med.clear();
        for (int i = 0; i < n; i++) {
            input(a);
            med.push(a);
            printf("%.1f\n", med.mediation());
        }
    }
    return 0;
}
```


