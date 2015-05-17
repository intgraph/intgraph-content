[Metadata]
title: Design a Distributed Random Number Generator
date: 2015-05-17 17:36:40

[Tags]
difficulty: 3.5
categories: distributed system
source: google

[Description]

Please design a distrubuted random number generator. The clients can get a random number from server by a single ``call()`` function.

[Solution]

![Alt text](http://wizmann-pic.qiniudn.com/d05dbd3183291e460f83434fdd36c851)

It's quite easy to build the skeleton of the system. Clients ask random number from the servers. And of course, it seems that everything goes well. But in a distributed system, everything MIGHT NOT goes well because a distributed system is prone to fail, or in the other word, is more possible to fail because the scale of the system is large.

There are several key points for the problem.

### Batching

It's known to us all that the network communication is much slower than fetching data from the memory. And this maybe the bottleneck for a high-performance application. It's wise to avoid network communication as possible.

Then, batching is a good choice. That is, the clients should ask a batch of random numbers from the server at one single time. And after the numbers are consumed, the clients ask for another batch again. The advantage of this method is, the time cost of one network communication is distributed over serverl random numbers, that will result for a high throughput of the system.

However, this design has one problem that the latency will be much higher than average for the request that cause a network communication. 

![Alt text](http://wizmann-pic.qiniudn.com/e43601d07ccea0325dede8cf72f82213)

That is unacceptable. The average response time is low, and it's fine. But this method suffers a performance jitter which may cause a bad user experience of the whole system.

There is another design. The client set a threshold of the remaining random numbers. If there are less numbers than the threshold, it will ask the server for another batch. This will do the trick to reduce the response time caused by network communication.

![Alt text](http://wizmann-pic.qiniudn.com/64cf2538460ae1ce320a7cd7d58e2901)

### Use a library rather than a proxy app

There is another design for the whole system. We use the proxy level to gain a better compability (maybe). But the disadvantage for an additional level, that is, if there is a proxy server in the middle, each message will pass the network twice. And you know, it's quite slow. And for a large scale system, additional proxy servers will increase the difficulty of deployments.

As a result, we just take a proxy-lib for the communications. It's light-weight, and more flexible. What is more important, it's more efficient (lower latency, high throughput).

![Alt text](http://wizmann-pic.qiniudn.com/c16ba0e9b1bc9bd13202020ff0eed8ed)

### Failure handling & Load balancing

As we discussed above, a distributed system is prone to fail. So a failure handling strategy is of vital importance.

#### Failure handling - Retry

Retry is one of the simplest failure handling strategy. All we have to do is to avoid attempts to communicate with unreachable peers. So the strategy is like this:

* Client consider a server is failed if the server doesn't respond to a request
* Client quickly discover that the server is unreachable, and then try to communicate with another replica
* Client periodically retires the failed server to detect if the server recovers or not

Advantages:

* simple and efficient, easy to implement

Disadvantages:

* prone to lost some requests when server fails

#### Failure handling - Sloppy Quorum

Slopppy Quorum is a method that could provide high availability when some of the replicas are not available. The clients ask for random number from serveral different servers rather than one single server in case of single node failure.

Advantages:

* high availability

Disadvantages:

* a little more complicated
* high network loads

### Lock free algorithm

Lock-free algorithm are simple mechanisms for inter-thread communication that don't relay on the kernel-provided synchronisation primitives, such as mutexes or semaphores; rather, they do the synchronisation using atomic CPU operations. This would provides higher performance for the server. And the lock-free queue is a nice practice to hand out the random numbers to the consumers.
