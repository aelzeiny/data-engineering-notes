# Hadoop
* horizontal scaling vs vertical scaling. Hadoop is multiple machines being one thing.
* Premise: Shipping data is expensive so let's put it in a "data ecosystem" & ship the computation closer to the data
* HDFS: distributed file cluster that acts like one hard drive
* YARN: yet-another-resource negotiator; like MESOS but for hadoop
* MapReduce: A mapper distributes data to multiple computers across the cluster; a reducer put it back together (even if a few servers fail)
* Pig: A procedural language that looks like SQL script, but translates that automatically to mappers and reducers
* Hive: A database that reads data with mappers and reducers

## HDFS
* Fault-tolerant, economical, higher throughput than just 1 FS.
* Files are replicated in a fault-tolerant way
* Master-Slave Architecture 
* Master = Name Node. Slaves = data nodes
### Name Node
* NameNode is what the client contacts. Data is chunked & split among data nodes.
* 2 name nodes b/c resilency
* Records any change to metadata like rename,create,remove,move.
* 2 files: EditLog & FSImage. One for edits the other for heirarchy.
* Metadata is stored in-memory for fast lookups
### Data Nodes
* Manage names & locations of file-blocks.
* config replication @ cluster/file lvl

## YARN
* Yet Another Resource Negotiator
* Used to be part of Map/Reduce but was split out
* Think Mesos. It's a scheduling layer that allows you to delegate jobs to free nodes w/i your hadoop cluster.
* Works with HDFS to run computations based on data locality
* Mesos vs Yarn: A node is allowed to accept/reject a job in Mesos.

## Map-Reduce
* 

## Tez
* Solves the same problem as Map-Reduce, but optimized for interactive queries
* Map Reduce is meant for batch workloads
* MR can be slow because it's always contacting HDFS & doing disk-reads + disk-writes

## Sqoop
* Simplified Data Transfer from HDFS to RDBMS
* Simplified Data Transfer from RDBMS to HDFS
* PRO: takes care of Hadoop provisioning, schema maintenance, with no code
* CON: Supported connectors ONLY