## Introduction to Redis
Source: https://redis.io/topics/introduction

Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache, and message broker. Redis provides data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams. Redis has built-in replication, Lua scripting, LRU eviction, transactions, and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.

You can run atomic operations on these types, like appending to a string; incrementing the value in a hash; pushing an element to a list; computing set intersection, union and difference; or getting the member with highest ranking in a sorted set.

To achieve top performance, Redis works with an in-memory dataset. Depending on your use case, you can persist your data either by periodically dumping the dataset to disk or by appending each command to a disk-based log. You can also disable persistence if you just need a feature-rich, networked, in-memory cache.

Redis also supports asynchronous replication, with very fast non-blocking first synchronization, auto-reconnection with partial resynchronization on net split.

Other features include:

- Transactions
- Pub/Sub
- Lua scripting
- Keys with a limited time-to-live
- LRU eviction of keys
- Automatic failover

## Data Types
- Strings: you can append, increment, decrement
- Lists: O(1) access to head and tail, O(n) access to other elements
- Sets: O(1) access, not repeated elements
- SortedSets, O(log(n)) access, sorted not repeated elements
- Hash: same as Python dicts
- Bitmaps and hyperlogslogs: Strings with more functionalites
