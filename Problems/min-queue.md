[Metadata]
title: MinQueue
date: 2015-10-14 10:01:22

[Tags]
difficulty: 4
categories: monotone
source: unknown

[Description]

Please design and implement a FIFO data structure which behaves like a queue, supports push, pop, front, and retrieving the minimal element in the queue.

* push(x) -- Push element x into queue.
* pop() -- Removes the element on the front of the queue.
* front() -- Get the first element of the queue.
* getMin() -- Retrieve the minimum element in the queue.

[Solution]

This problem is similar to the "MinStack" which is very known by most of us. But when we try to solve the problem in the same way, we will find out that it's quite impossible to maintain the information of the minimal element either in a stack or a queue.

Suppose we've got a queue with elements, and one single variable with the minimal element.

* Case 1: push(x)      
It's OK to push something in this data structure, if the new element is greater than the current minimal value, we just put it right after the current minimal value. Because if the minimal value is popped out, the new element can be the potential one to be the new minimum. If the new element is less than the current minimum, we just replace it. Because when we pop the element out, the current minimum will be popped before our new element.

* Case 2: front()     
Behave like a ordinary queue.

* Case 3: min()      
Just return the minimal value.

* Case 4: pop()     
This is the hardest one in our scenario. Because we have to maintain the property of our data structure to make everything right. If the element we pop from the queue is greater than the current minimal value, we just remove it from the queue, and keep the minimal value because the minimal element is still in the queue. If the element we pop is equal to the minimal value, we will replace it by the new minimal value in the rest of the queue.

It's an intuitive thought with half-baked idea. But it indicate the principle of the design of our data structure. First of all, the container we maintain the minimal value should be a deque, because we have to push/pop the element from the back of it, at the same time, pop things out from the front is necessary for the data structure. Secondly, the deque will be in increasing order, because if `dq[a] > dq[b] && a < b`, then `dq[a]` should replaced by `dq[b]`.

So our mission is to keep a deque with increasing order to maintain the information of the minimal value. And we call this as the "monotone priority queue".

The code is like this:

```python
import abc
import random
from collections import deque

class MinQueueBase(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def push(self, item):
        pass

    @abc.abstractmethod
    def front(self):
        pass

    @abc.abstractmethod
    def pop(self):
        pass

    @abc.abstractmethod
    def min(self):
        pass

    @abc.abstractmethod
    def empty(self):
        pass

class MinQueueTest(MinQueueBase):
    def __init__(self):
        self.q = []

    def push(self, item):
        self.q.append(item)

    def front(self):
        if not self.q:
            return None
        return self.q[0]

    def pop(self):
        if not self.q:
            return None
        u = self.q[0]
        del self.q[0]
        return u

    def empty(self):
        return False if self.q else True

    def min(self):
        if not self.q:
            return None
        return min(self.q)

class MinQueue(MinQueueBase):
    def __init__(self):
        self.q = deque()
        self.mini = deque()

    def push(self, item):
        self.q.append(item)
        while self.mini and self.mini[-1] > item:
            self.mini.pop()
        self.mini.append(item)

    def front(self):
        if not self.q:
            return None
        return self.q[0]

    def pop(self):
        if not self.q:
            return None
        u = self.q.popleft()
        if u == self.mini[0]:
            self.mini.popleft()
        return u

    def empty(self):
        return False if self.q else True

    def min(self):
        if not self.q:
            return None
        return self.mini[0]

mqtest = MinQueueTest()
mq = MinQueue()

cmds = ['push', 'pop', 'front', 'empty', 'min']

for i in xrange(10000):
    cmd = random.choice(cmds)
    if cmd == 'push':
        value = random.randint(-100, 100)
        print cmd, value
        mqtest.push(value)
        mq.push(value)
    else:
        a = getattr(mqtest, cmd)()
        b = getattr(mq, cmd)()
        print cmd, a, b
        assert a == b

```
