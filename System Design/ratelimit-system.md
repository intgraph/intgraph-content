[Metadata]
title: Design a Rate-limit System
date: 2015-08-31 01:09:45

[Tags]
difficulty: 3
categories: distributed system
source: unknown


[Description]

Design a rate-limit system for our website that block user when the requests are more than 10/min or 100/hour or 1000/day...

[Solution]

### Requirements analysis

| Requirements | Simplified Solution |
| --- | --- |
| Block malicious users(*) | Count the number of user requests in a time period |
| Collect the request information | Use MQ or just collect the log file from the service |
| Deal with big data | the system must be scalable and should optimize the resources usage |
| Realtime | the delay of the system should be as short as it can |

### Lv1. Smoke test - rate-limit System on a Single Machine

I'll write a sample code to describe the system.

```python
import time
from collections import deque

class RatelimitSystem(object):
    def __init__(self):
        self.dq = deque()
        self.counter = dict()

    def query(self, user_id):
        self.adjust()
        self.add_record(user_id)
        if self.counter[user_id] < THRESHOLD:
            return OK
        else:
            return BLOCK_IT

    def adjust(self):
        cur = self.cur_time
        while dq:
            (usr, tm) = dq[0]
            if cur - tm >= THRESHOLD:
                dq.popleft()
                self.counter[usr] -= 1
            else:
                break
        return SUCCESS

    def add_record(self, user_id):
        self.counter[user_id] =
                self.counter.get(user_id, 0) + 1
        dq.append((user_id, self.cur_time()))
        return SUCCESS

    def cur_time(self):
        return time.time()

```

That is, every user requests should be send to this system, call `query(user_id)`, then we will return if this user is a friendly one or a malicious one.

We can use MySQL as the storage engine while Redis(or memcache) is better. Because Redis offers better performance and the builtin deque data structure.

## Lv2. Optimize and Trade-offs

### Collect the information

In the simplified system above, we skip the information collect part which is essential for our system.

The very first step for the information collecting is to design a interface(or prototype) for this part.

```cpp
class ICollector {
    void collect() = 0;
    int status() = 0;
};
```

We can design the collector for HDFS, or using a MQ to get the request information from the service directly. If the service can't offer us an interface for these information, it's OK to collect the log from the hard disk as a fallback plan.

### Optimize the Resources Usage

If we put all the user request information to the main memory, sometimes it needs a lot of space that we can't afford.

Here we use the LRU (Least Recent Use) strategy to move some less likely malicious user from the cache to storage them in the hard disk (or SSD) which is larger, slower and cheaper. And we aggregate the information in the cache to reduce the visit time of database which is slow.


![](http://wizmann-pic.qiniudn.com/15-8-31/19748924.jpg)

### Realtime or Half Realtime

At the solution above, we design a "full realtime" rate-limit system, for example, a user sends a request on time `t`, we will check if this user have too many request between time `[t - THRESHOLD, t]`.

To implement this, we have to storage the all the request  information in the time period in a deque. But if we change the definition of a "time period" we can safe a lot of memory without the deque.

Assuming there is a user request at "08:30", then we just check whether the request number is exceed the limitation in "[08:00, 09:00]".

That is, we refresh our rate-limit records every one hour in this example. And simplify the system by removing the request information deque.

## Lv.3 Distribute it

The distributed rate-limit system is similar to the single machine version. We just use cluster of brokers and ratelimiters for collecting more information that can't be handled by one single machine.

![](http://wizmann-pic.qiniudn.com/15-8-31/7688908.jpg)
