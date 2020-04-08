# Redis the Single-Threaded Swiss Army Knife

## Data Structures
Redis supports
* String
* Sets
* Sorted Set
* List
* Hashes
* Bitmaps
* Bitfield
* HyperLogLog
* Geospatial indexes
* Streams


## Swiss Army Features
* Pub-Sub
* Queues
* Streaming
* Module: RediSearch. Redis search Engine
* Module: rediSQL. in-memory SQL lite.
* Module: In memory graph DB

## Eviction Policies

In summary - Redis can evict least recently used (LRU), least frequently used (LFU), and random records from a group. Redis can consider all keys, or only keys that are set to expire. VolatileTTL is a combo of LFU & least time left to live. No Evicition means exactly what it says. [Link to docs](https://docs.redislabs.com/latest/rs/administering/database-operations/eviction-policy/)

* **noeviction**
* **allkeys-lru**
* **allkeys-lfu**
* **allkeys-random**
* **volatile-lru**
* **volatile-lfu**
* **volatile-random**
* **volatile-ttl**

## Cache Invalidation
#### Write-through cache
Write once onto persistant storage, and another onto redis.


**Observation**: Set eviction policy based on use-case, but probably not "noevict"

**Pros**:
* Resilient.
* Simple Concept

**Cons**:
* Additional complexity in each write
* Writing everything twice has latency.

#### Write-around cache
Write data directly to perminant storage. On reads, check the cache & populate if cache misses.

**Observation**: Set eviction policy based on use-case, but probably not "noevict"

**Pros**:
* Only serialize once to persistant DB
* Only cache what you need 

**Cons**:
* Cache Misses
* even scarrier, Stale Cache!

#### Write-back cache
Write directly to cache & cache only. Persist to DB async. Maybe have cache replication to reduce failures.

**Observation**: Set eviction policy to volatile. When the initial cache write occurs, add to cache with no expiration. When persistant write occurs, set expiration on the record. That way, no evictions happen for data that is not written unless node fails.

**Pros**
* Scales up writes

**Cons**
* You play a risky game. Nodes can fail.

## Paritioning
Before there were other methods for distributing Redis. In 2015 Redis Cluster became defacto standard. Doesn't use consistent hashing, just hash slots. Take the key, hash it, find the machine with that key.

Contact a random node, request is forwarded to the right node based off of query. This is called Query Routing, because you're choosing the cluster based off of hash key.

When adding or removing a node, data is rebalanced. Redis Cluster allows for no downtime.

Master-slave failover is an option

No strong consistency in any failover/configuration.

[See Docs](https://redis.io/topics/partitioning)