# Spark Concepts

### Map Reduce Review
* 3 operations: map, shuffle/sort, reduce
* slow due to replication, serialization, and disk IO
* Standard Map-reduce job could take 90% of the time HDFS read-write operations

### RDD Review
* An in-memory datastructure that prevents write & reads from disk
* If memory is insufficient, rather than resorting to swap, RDDs will be writen to local disk
* You can persist/cache an RDD in-memory for faster operations

### Spark-SQL Review
* Don't like RDDs. Try Dataframes.
* Makes spark a bit more sql-like for easier processing. 
* Abstract out data-sources with Data Source API (i.e: Hive, Avro, any file, JDBC dbs, etc.)
### Spark Streaming
#### Streaming Context
* 

meh, i kinda already know this stuff. Notes to be added later.