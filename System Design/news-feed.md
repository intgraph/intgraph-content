[Metadata]
title: Design a News Feed System
date: 2015-08-30 15:51:17

[Tags]
difficulty: 3.5
categories: distributed system
source: facebook

[Description]

Design a news feed system like facebook.

![](http://wizmann-pic.qiniudn.com/15-8-30/69933929.jpg)

[Solution]

## Lv.1 - Smoke Test - News feed system on a single machine

### What is a feed?
```
// @ the interface of news feed
class IFeed {
public:
    uint32_t user_id;
    time_t   post_time;

    Content content() = 0;
};
```

The `content` can be a simple tweet, a blog, a url or a picture.

### The Database Schema

We use SQL database as the backend storage engine. So we can design our database schema for the newsfeed system.

| UserID | Name | Age | Sex |
|-|
| 1 | Alice | 20 | F |
| 2 | Bob | 21 | M |

| FriendshipID | FromUser | ToUser |
|-|
| 1 | 1 | 2 |
| 2 | 2 | 1 |

| FeedID | UserID | Time | ContentID |
|-|
| 1 | 1 | 2015-01-01 | 1 |

| ContentID | Type | Content |
|-|
| 1 | Tweet | "hello world" |

### Get the newsfeed

```python
friends = FriendTable.get(id=usr_id)
feeds = FeedTable.get(
        user_id__in = friends,
        sort_by__reverse = time
)[OFFSET:OFFSET + LIMIT]

# some filter and sorting logic

for feed in feeds:
    feed.content = ContentTable.get(id = feed.content_id)

return feeds
```

## Lv2. - Single Machine System with Some Optimization

### Use cache to decrease the time of database acessing

As described above, each request that users make will access the database. It's known to us that database access is not that fast. And prone to be crashed if there are too many queries at same time.

One good practise to speed up the database accessing is to use cache. The advantages of the cache are:

1. Cache is a faster memory based storage service than database
2. Cache can decrease the time of database accessing, help to reduce the pressure of our database system
3. Cache is light-weight system that only take availability, scalability. That is we can have a quite flexible system for us to handle all the requests.

### Using Mailbox to speed up user requests

We design a mailbox services that collects all the news feed that belongs to one user to his/her mailbox.

| UserID | FeedIDList |
|-|
| 1 | [2, 3, 4 ...] |
| 2 | [1, 4, 10 ...] |

That is, if a user request for his news feeds, we will get the list of the news feeds. Then we use the feed ids to get the contents itself, and do some tricks of filtering, sorting or something else. After all, we show news feeds to users.

But how to design a way to keep a realtime mailbox that the information of the mailbox is up-to-date.

There are two ways to maintain a realtime mailbox.

* Push
If Alice post something, her news feeds will propagate to the mailbox of all her friends. So if Bob is online, he will get the information from Alice from his mailbox.
* Pull
Pull mode is similar to "using the cache" as we described above. Everytime Alice get online and ask for her newsfeed, our service will update her mailbox.

There are advantages and disadvantages of this two method. And we will discuss it later.

## Lv3. - Distributed it

### The design of the system

![](http://wizmann-pic.qiniudn.com/15-8-29/65565884.jpg)

### Load balancing

"NewsFeedProxy" & "RequestCommander" & "ResponseCommander" are three service which contains load balancing functionality that make the downstream system efficiently.

We can use **random**, **round robin** or **weighted round robin** to balancing the loads.

### Health check & Failure Recovery

If a single server doesn't response the requests, for example, timeout, connection closed. It shows that the server is "not healthy", but the reason is diversed, for example, network failure, server failure, high loads on the server, etc.

These problem maybe temporary or permanent, so we use a health check algorithm to keep the list of the available services. For example, if service node A is "unavailble" at time `t`, then we put it into the "blacklist" until `t + delta_t`. If the service node is still unavailable, then we put it in the blacklist for a longer time until it meets the maximal retry time, for example `t + min(MAX_RETRY_TIME, k * delta_t)` and `k` is stand for the failure times.

If a service node has already recovered, then we will put this node to the "available service node list" again.

### Data Handling

#### Support Modification & Deletion

In our architecture, the modification and deletion are very simple because we can modify / delete the news feed in the "NewsFeedSerivce", and the "MailBoxService" will no nothing about it.

#### Memory Optimization

As described above, there is a service handling all the news feed. And if there are all data in a same kind, it's easy to optimize the memory usage.

* Separate the "hot data" and the "cold data"
if one feed is not requested by user for a couple of days, we can temporarily put it to the SSD / hard disk to save the memory space.
* Deduplicate and compression
using deduplicate and compression the string could save memory up to 90% (this data is from leveldb of Google).

### The comparison between the "push" and "pull"

| - | Push | Pull |
| - |
| real-time | OK except some "star users"| Good |
| server status | keeping pushing status (SUCC/FAIL) | N/A |
| client status | N/A | keep current data |
| storage | centralize on feed server | distributed to all clients |
| load balance | by server | by client |
