Ben Stopford - Elements of Scale

http://highscalability.com/blog/2015/5/4/elements-of-scale-composing-and-scaling-data-platforms.html

# Storage Engines
### Let's start with a stupid DB:
* It's one file. One table. Can do sequential reads from disk
* SEQUENTIAL Reads from disk > RANDOM accessed memory sometimes
* Say you have to update. MySQL will update and modify a record and has buffer space to compensate
* Postgres will do append-only; which is super-fast in terms of write.
* "Indexing = overarching structure". Meaning random writes to disk. Oh no.

### (A) Let's say you put index in memory (Mongo, Rabbit)
* Now if we add index; we slow down writes. drats.
* We COULD keep our index in memory. But what if we have too many indexes than memory? Oh no.

### (B) So enter Log-Structured Merge Trees (HBase, Cassandra)
* Collection of small indexes instead of 1 big one.
* Append-only writes. Occassionally I'll merge 2 files into one file.
* WRITES: Faster because now I'm not scanning in my entire index file.
* READS: Slower because I don't know which index file is best index file.
* READ OPTIMIZATIONS: In-memory metadata store w/Bloom filters.
* Recall that bloom filters are probablist data structures that you can tune to some probablity. Gives true negatives and false postives.

### (C) Columnar stores be like (RedShift, Parquet)
* Less IO, each column is compressed
* Held in row order, merge joins via rowid (pd Dataframe style)
* Predicates can operate on compressed data
* Late materialization

# Partitioning/Sharding
* Spreading data across multiple sets of machines

### Consistent Hashing:
* limited. not all problems apply like this.
* PRO: quick direct access. 
* PRO: truly linearly scalable. Truly adding another machine = proportion throughput or storage
* CON: Secondary indexes suck, because if the shards are based on PK hash; you have to contact every node for a secondary key.
* HBase has no Secondary index as a result. Cassandra & Mongo do, but that's a problem.

### Batch/Divide and conquer
* Kinda like map-reduce. You map out the problem to divide up the problem to multiple machines in broadcast.
* PROS: Good for big computations & long running jobs
* CON: Concurrency suffers as a result

### Replication
* replicatas can be invisible (fault recovery), read-only, or RWs (partition tolerance).
* Good when you have to broadcast load (like map-reduce)
* Bad when consistency is a problem. Think CAP & Tradeoffs to CAP.
* Atomicity (All writes look like they took place on 1 thread) is also a super expensive in distributed world.

#### Solution 1: Leader-follower
* This avoid the problem.

#### Solution 2: Client-Query Response Segreation (CQRS) (Druid)
* Take a (write) command, write it to a write-optimized DB.
* Denormalize/precomputed to a read-optimized db that queries go out of.
* Pro: good for reads & writes
* Cons: You can't just read what you just wrote. Preproc takes time.

#### Solution 3: Seperate Mutable from Immutable with Streams
* Front-section provides synchronized reads/writes with any DB. 
* Back-section use stream processors to populate views across multiple different data models 
* The backend DB is immutable & has read-only views. 
* PRO: Therefore it's easy to scale.
* PRO: Connect streams up to other clients (i.e: notification consumers)
* PRO: If a data-model changes in backend, replay the stream. Best migration ever!
* CON: maintaining clean data across multiple data models & formats
* Notes from: www.benstopford.com/2015/04/07/upside-down-databases-bridging-the-operational-and-analytic-worlds-with-streams/

#### Solution 4: Batch Pipelines & Hadoop
* Scoop & Flume throw things into your HDFS
* Oozie/Airflow moving things around as orchastration
* Hive/Impala/Kudu/Presto pulling data from HDFS
* Batch job loading data from HDFS into DB.
* Very LeanTaaS. Very Hadoop.
* Pro: great for immutable data

#### Solution 5: Lambda architecture (what I do at LeanTaaS)
* Streaming layer with a window will give us fast (i.e: HL7), but inaccurate results
* Batch layer will eventually overwrite with best results 
* Pro: works well with immutable data (Regenerate state from any point of time)
* Con: dual code for batch & live. You can use Flink to combine, but usually only really masks the issue.

#### Solution 6: Streaming Data platforms
* I think of stream data as a superset of batch
* Deal with data eagerly as soon as it arrives
* stream in Kafka because it's kinda already a DB. Perhaps use kafka's ecosystem for simple workers.
* Throw into views

# Takeaways:
* Immutablility is cool. Treat state like immutable pieces of data over time
* Appending writes to an immutable stream is the best
* Reactive systems with streams is better than batch
* Replays are great w/Immutablility because it can regen state & regen downstream views when those change
* Seperate mutable (Read & write) from immutable (just read)
* create read-replicates around immutable