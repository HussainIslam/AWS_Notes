# DynamoDB

## Intro
* serverless NoSQL distributed database
* fully managed by AWS
* highly available with replication in 3 AZ
* horizontally scales
* integrates with other serverless services
* millions of requests per second, trillions of rows, 100s of TB of storage
* fast and consistent in performance (low latency)
* integrated with IAM
* enable event driven programming with dynamoDB Streams
* low cost and auto scales

## Basics
* made of tables
* each table has primary keys
* each table can have infinite number of items/rows
* each item can have attributes (similar to columns but same)
* attributes can be added over time and can be null
* max size of a row is 400KB
* Data types supported:
	* scalar types: string, number, binary, boolean, null
	* document type: list, map
	* set types: string set, number set, binary set
* Primary Key:
	* Option 1: Partition Key only (HASH)
		* must be unique for each item
		* it must be diverse enough so that data is distributed
	* Option 2: Partition Key + Sort Key 
		* unique combination
		* grouped by partition key
		* sorted by sort key also called range key

## Throughputs
* Provisioned Throughput
	* must have provisioned read and write units
	* Read Capacity Units (RCU)
	* Write Capacity Units (WCU)
	* option to enable auto-scaling
	* throughput can be exceeded temporarily by using "burst credit"
	* if burst credit are empty, get "ProvisionedThroughputException"
	* do exponential backoff retries
* Write Capacity Units
	* 1 WCU equal to one write per second for an item up to 1KB
	* more WCU consumed for over 1KB
	* Example: We need to write 10 objects per second of 2KB each. So, we need 2 * 10 = 20 WCU
* Read Capacity Units
	* 1 RCU equals 1 strongly consistent read per second or 2 eventually consistent reads per second for an item up to 4KB
	* more RCU are consumed for items over 4KB
	* Example: 16 eventually consisten reads per second of 12 KB each (16/2 * 12/4 = 24 RCU)
* Consistency
	* Eventually Consistent Read: reading immediately after writing may result in unexpected response; default option; affects RCU
	* Strongly consistent read: always get the correct data
* Internal Partitions
	* data is divided into partitions
	* parition keys go through a hashing algorithm to know to which partition they go to
	* to compute the number of partitions:
		* By Capacity: (TOTAL RCU / 3000)  + (TOTAL WCU / 1000)
		* By Size: Total Size / 10 GB
		* Total Number of partitions: Rounded maximum of Capacity and Size
	* RCU and WCU are spread evenly between paritions
* Throttling
	* if WCU or RCU exceeds, ProvisionedThroughputExceededExceptions
	* Reasons:
		* Hot keys: one parition key being read too many times
		* hot partitions
		* very large items
	* solutions:
		* exponential backoff
		* distribute partition key as much possible
		* if RCU issue, we can use DynamoDB Accelerator (DAX)

## DynamoDB APIs
* PutItem: create or replace items
* UpdateItems: partially update attributes; atomic counters to increase count of an item
* Considtional Writes: write/update on condition; concurrent access to items; no performance impact
* DeleteItem: delete an item; can add condition
* DeleteTable: deletes table and all items
* BatchWriteItems: up to 25 PutItem and/or DeleteItem in one call; 16MB write and 400KB per item; saves latency; done in parallel; can retry failed items
* GetItem: read based on primary key; eventual consistent by default; **ProjectionExpression** to get specific attributes
* BatchGetItem: upto 100 items; upto 16MB; retrieved in parallel
* Query:
	* efficient way of querying
	* PartitionKey value
	* SortKey value (=,<,<=,>,>=,Between, Begin) (optional)
	* FilterExpression to further filter (done client side)
	* returns upto 1MB
	* can Limit number of items
	* able to do pagination on results
	* can query table, a local secondary index, or a global secondary index
* Scan:
	* inefficient way
	* scans entire table and filter data
	* returns up to 1MB
	* can use pagination
	* consumes lot of RCU
	* limit impact of Scan by limiting size
	* faster performance by parallel scan (deplets RCU)
	* can use ProjectExpression + FilterExpression

## DynamoDB CLI
* `--projection-expression`: list of attributes to retrieve
* `--filter-expression`: filter results
* `--page-size`: optimized page size
* `--max-items`: for pagination
* `--starting-token`: last received Next Token to keep reading in pagination
* 

## Secondary Index
* Local Secondary Index
	* Alternate range key for table; local to hash key (partition key)
	* can have upto 5 seconary indexes
	* sort key must be one scalar attribute
	* attribute chose must be scalar string, number or binary
	* must be defined at table creation time
	* can't be created/edited after table creation
	* uses the WCU and RCU of main table
	* no special throttling considertions
* Global Secondary Index
	* to speed up queries on non-key attributes
	* GSI = new partition key + optional sort key
	* can be considered as a "table" and can project attributes on it
		* partition key and sort key of original table are always projected (KEYS_ONLY)
		* can specify extra attributes to project (with INCLUDE)
		* can use all attributes of main table (with ALL)
	* define RCU and WCU
	* possible to add/edit GSI
	* If writes are throttle on GSI, the main table will also be throttled
	* choose GSI partition key carefully
	* assign WCU on GSI carefully
* Additional resource: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-indexes-general.html

## Optimistic Concurrency
* DynamoDB can do conditional update/delete (can ensure that item hasn't changed before changing it)
* That makes DynamoDB **Optimistic Locking/concurrency** database

## DynamoDB DAX (Accelerator)
* This is an caching technology
* this is seamless, meaning no need to rewrite application
* DynamoDB write through DAX and adds microsecond latency for cached reads and queries
* Solves Hot Key Problem (too many reads)
* every query will be cached for 5 minutes
* upto 10 nodes in cluster
* Multi AZ (3 nodes minimum recommended for production)
* secure (at rest: KMS, Network VPC, access IAM, logged with CloudTrail)
* not free tier

## DynamoDB Streams
* changes in the DB go to the Stream
* can be read by AWS Lambda, EC2 to:
	* react to changes
	* analytics
	* create views
	* insert into ElasticSearch
* can implement cross region replication using Streams
* only 24 hour data retention
* choose what information being modified:
	* KEYS_ONLY: send only the keys
	* NEW_IMAGE: item after modification
	* OLD_IMAGE: item before modification
	* NEW_AND_OLD_IMAGES: both new and old items
* made of shards
* don't manually provision shards, automatically done by AWS
* data is not retroactively populated in stream

## DynamoDB and Lambda
* need to define Event Source Mapping to read from DynamoDB streams
* need to make sure Lambda has appropriate permissions
* lambda function is invoked synchronously

## Additional Concepts
* TTL (Time to Live)
	* define an expiry based on the value of a column
	* no extra cost
	* background task by DynamoDB
	* helps reduce storage and table size
	* helps adhere to regulatory norms
	* enabled per row basis
	* will try to delete within 48 hours of expiration
	* data will be deleted from GSI/LSI
	* Streams can help recover deleted items
* Transactions
	* ability to create/Update/Delete multiple rows in multiple tables
	* "all or nothing" type of operation. either all or none of them happen
	* Write Modes:
		* Standard
		* Transactional
	* Read modes:
		* Eventual consistency
		* strong consistency
		* transactional
	* Consumes 2 times WCU and RCU
* Session State Cache
	* common to use store session state
	* vs ElastiCache:
		* fully in memory vs DynamoDB is serverless
		* both are key-value stores
	* vs EFS:
		* EFS must be attached to EC2
		* EFS is file system
		* EFS is storage
	* vs EBS and Instance Store:
		* these can be used for only local caching not shared caching
	* S3:
		* higher latency
		* meant for large objects
* Write Sharding
	* add a random/calculated suffix to distribute Partition key
	* helps to scale up
* Write types:
	* Concurrent writes: first person will write; second person will overwrite first person
	* conditional writes: write is based on a condition; second write will fail
	* Atomic writes: two users increase/decrease by different amounts; both succeed
	* batch writes: many items at a time
* Patterns with S3:
	* Large Objects Pattern
		* maximum object size is 400KB
		* upload to S3 first and insert the metadata to DynamoDB
	* Indexing S3 Objects metadata
		* upload files to S3 and add Event notification to it
		* the event notification will call Lambda function and insert metadata to DynamoDB table
		* DynamoDB table data can be indexed for faster access
* Operations:
	* Table Cleanup: 
		* Option 1: Scan + Delete; slow and expensive
		* Option 2: Drop table + Recreate Table; fast and cheap
	* Copying DynamoDB Table:
		* Option 1: AWS Datapipeline (using EMR, big data tool); export to S3 and then import to another DynamoDB table
		* Option 2: create a backup and restore (most efficient)
		* Option 3: Scan + Write (least efficient)
* Secuirty:
	* VPC Endpoints for access without internet
	* access controlled by IAM
	* Encyption at rest KMS
	* Encryption at transit with HTTPS
* backup and restore:
	* point-in-time restore; like RDS
	* No performance impact
* Global tables:
	* globally available
	* multi region
	* fully replicated
	* high performance
* Amazon DMS
	* migrate data to DynamoDB from:
		* MongoDB
		* Oracle
		* MySQL
		* S3
* DynamoDB can be launched locally on computer