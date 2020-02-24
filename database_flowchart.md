# Database Flowchart
Basic litmus test for data requirements at a company

## Cap Theorem 
* Consistency
* Availability
* Partition Tolerance

## R/W Questions
* Frequency of Reads?
* Latency of Reads?
* Frequency of Writes?
* Latency of Writes?
* Size of data?

## Primary & Secondary Indexes
* Do I have a lot of secondary indexes?

# Tricks
* No parition tolerance? Relational DB
* No availability or analytical workloads or low latency of reads? Presto/Hive/Impala/Kudu
* High Frequency of writes w/secondary indexes w/partition tolerance? You're in trouble. Consider using a DB with Log-Structured-Merge Trees (Cassandra)
* High Frequency of Reads w/secondary indexes? B+ Trees or In-memory.