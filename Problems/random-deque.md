[Metadata]
title: Random Deque
date:  2015-07-09 09:33:05 

[Tags]
difficulty: 4
categories: deque, induction
source: mitbbs, anonymous

[Description]

Here we have a data structure called "random deque". It's "random", because you can call the function `peak()` to get the element from the front or back of the deque, and the behavior of this function is **unpredictable**.

This is the interface of this data structure.

```cpp
template <typename T>
class RandomDeque {
public:
    // put an element to the back of the deque
    void push_back(const T& ele); 
    
    // randomly get an element from either the 
    // front or the back of the deque
    T peak();
    
    // pop out the "peak" element, which is the same
    // "peak element" that get from the previous function
    // call of ``peak()``
    void pop();
    
    void size();
    void empty();
};
```

Here we have a random deque `RandomDeque<int>`, and the elements in it is sorted, and all elements is unique. Our mission is to copy all the elements from the deque to an array with order. The time complexity for this problem should not exceed `O(n)`, and no extra memory space should be used.

Follow ups:

1. If there are duplicated elements in the deque, how can we deal with it properly?


[Solution]

At the very beginning, I will show you how to implement the `class RandomDeque`. All the codes below are based on this data structure.

```cpp
template <typename T>
class RandomDeque {
public:
    RandomDeque(): prev(0) {}
    RandomDeque(int n): prev(0) {
        for (int i = 0; i < n; i++) {
            dq.push_back(T(i));
        }
    }

    void push_back(const T& ele) {
        dq.push_back(ele);
    }

    T peak() {
        prev = rand() % 2 == 0? 1: -1;
        return prev == 1? *dq.begin(): *dq.rbegin();
    }

    T pop() {
        if (prev == 0) {
            peak();
        }
        if (prev == 1) {
            T ele = *dq.begin();
            dq.pop_front();
            return ele;
        } else {
            T ele = *dq.rbegin();
            dq.pop_back();
            return ele;
        }
    }
    
    bool empty() {
        return dq.empty();
    }
    
    size_t size() {
        return dq.size();
    }
private:
    int prev;
    deque<T> dq;
};
```

The first problem is not hard. The intuitive thought is call the function `peak()` for multiple times. And then we can get the first element and the last element.

This method do not guarantee we can make everything right in everytime. But it's prone to be right. 

For example, for an deque with 10,000 elements, we try to call `peak()` 20 times each time we try to get one single element. And the possibility we can get the right answer is greater than 99%. If we try 100 times, and the errer probability is less than `1e-26`.

```cpp
int main() {
    srand(time(NULL));
    int cas = 0;
    while (true) {
        cas++;
        int N = 10000;
        RandomDeque<int> rd(N);
        vector<int> vec;

        while (!rd.empty()) {
            int a = rd.peak();
            int b = a;

            for (int i = 0; i < 100 && b == a; i++) {
                b = rd.peak();
            }

            if (a == b) {
                vec.push_back(a);
                break;
            }
            if (a > b) {
                swap(a, b);
            }
            vec.push_back(a);
            while (rd.peak() != a) {
                // pass
            }
            rd.pop();
        }

        bool flag = vec.size() == N;
        for (int i = 0; i < N && flag; i++) {
            if (vec[i] != i) {
                flag = false;
            }
        }
        if (!flag) {
            break;
        }
        if (cas % 1000 == 0) {
            print("Test success @ test" << cas);
        }
    }
    print("Test failed @ test" << cas);
    return 0;
}
```

The time complexity of this problem is `O(100 * N) => O(N)`, and the error rate is very low. It's a good solution for the first question. But it's not good enough.

If the random function behaves strangely that will not give random number with equal possibility. For example, the probability that `rand()` gives us `0` is 99.99% and `1` is 0.01%. The error probablity of the solution will be quite high. We have to find out a deterministic to avoid that situation. 

Think about this situation. We have got an "peak" element, it may from the front or the back of the deque. If it is from the front, every other elements is greater than this one. Otherwise, if it is from the back, every other ones is less than this one. Then, we put this element to the proper position of the result array.

Of course, this solution can be prove to be right. And it's quite easy to prove, but not too easy to come up with.

```cpp
int main() {
    const int N = 10000;
    RandomDeque<int> rd(N);
    vector<int> vec;
    vec.resize(N);
    int p = 0;
    int q = N - 1;
    
    while (!rd.empty()) {
        int a = rd.peak();
        rd.pop();
        
        if (rd.empty()) {
            vec[p] = a;
            break;
        }
        int b = rd.peak();
        
        if (a < b) {
            vec[p++] = a;
        } else {
            vec[q--] = a;
        }
    }

    bool flag = vec.size() == N;
    for (int i = 0; i < N && flag; i++) {
        if (vec[i] != i) {
            flag = false;
        }
    }
    
    if (flag) {
        print("Test success!);
    } else {
        print("Test failed!);
    }
    return 0;
}
```

For the follow-up question, we just add one counter for the "peak" element, to mark how many elements are equal to the "peak" one.
