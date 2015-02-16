[Metadata]
title: IP to location
date: 2014-08-27 23:14:09 

[Tags]
difficulty: 2.5
categories: hash, data structures
source: baidu

[Description]
As a ads system, sometimes we have to get the location of users to tickle them with some better ads. (But who likes ads, right?)

Now, we have a table of some **IP segments** and the corresponding locations (country, province, city or district).

Your mission is to build a data structure or an algorithm to get the location by a given **IP address**.

[Solution]
First of all, we have to convert the IP address (IPv4 in here) which is string into an integer. That will lead to a reduction of the memory usage.

Secondly, we can't use a simple hash algorithm because one IP segment contains thousands (or more) IP addresses, and some IP segments are reserved. So the "IP -> Location" table is quite sparse, and it waste a huge amount of memory space if we store the information of every IP in this scenario.

And there are two solutions here. One is simple, and the other is a bit complex. (Easy enough after all.)

### Solution No. 1

It's easy to find out that we can split the IP address into two ``uint16_t`` numbers, ``ip_addr = <L, R>``. And the IP adresses which share the same location is usually continuous.

Assuming we have a IP segment starts from ``<L, R1>`` and end with ``<L, R2>``. We take the ``<L>`` part as key, and take the ``<R1, R2>`` part as value to make a hash table. And if we get a IP address, we can seek which segment it is in from the hash table with a **not-bad** performance.

If a continuous IP segment starts from ``<L1, R1>`` and end with ``<L2, R2>``, we can split it to two or more segments which share the same ``<L>`` part.

> I won't show my code here because I'm sleepy. (-.-)zzz

### Solution No. 2

As the IP address can be represented as a ``uint32_t`` number, we can make the IP segments as ``<uint32_t, uint32_t>`` pairs. And we sort these pairs by the second element.

When we're seeking a specific IP address, we just use the binary search (lower_bound) to find which segment the IP address is in.

> Sleepy. (-.-)zzz

### Some other solutions

* Segment Tree

Not a bad choice, but it's too heavy in this scenario.

* Binary Indexed Tree

Is that actually necessary?

* Linear Scan

Rough, Fast, Fierce. (糙快猛 in Chinese)
