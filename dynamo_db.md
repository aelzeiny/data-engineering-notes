# Dynamo DB
* Type: Document Store
* Serverless
* Schemaless (like most document stores)

## Components
* Table - collection of data
* Items - row/collection/document of one element w/i a table.
* Attribute - keys in an item. The values can be 'scalar' or 'nested' like a JSON.

## Concepts
### Primary keys
#### Standard
* Partition Key - key-value style lookups which returns an item
* table defines a "hash attribute".
* I think this probably uses consistant hashing underneath the hood.
#### Composite
* Partition Key and Sort/Range Key
* each document is partitioned by the partition key. Then each file is sequentially sorted based off of sort/range key.
* Good for scans.
* Combination of partition & sort keys must be unique
### Secondary Index
* Global secondary index - both partition and sort keys are different from table
* Local secondary index - same partition key; different sort key

### DynamoDB Streams
* Each time an item is added/updated/deleted it will induce a change-capture
* AWS Lambda can trigger off of change-capture.

### Atomic Counters
* An asynchronous way to increment to a counter. No need for an ack.
* Good for use-cases where consistency barely matters.