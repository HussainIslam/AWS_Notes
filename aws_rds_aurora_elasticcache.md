# Relational Database Service (RDS)

## Relational Database Service (RDS)
* Managed DB service and use SQL
* RDBMS supported by AWS:
	* Postgre
	* MySQL
	* MariaDB
	* Oracle
	* MSSQL
	* Aurora
* Advantages:
	* managed service
		* automated provisioning
		* continuous backups and restore
		* monitoring dashboard
		* read replicas for improved read per instance
		* muti AZ setup for DR (Disaster Recovery)
		* Maintenance windows for upgrades
		* Scaling capability (vertical and horizontal)
		* Storage backed by EBS (gp2/io1)
* Can't SSH into these
* RDBMS client: https://github.com/sqlectron/sqlectron-gui/releases/tag/v1.32.1

## RDS Backups
* Automated backups
	* daily full backup
	* transaction logs are backed up every 5 minutes
	* ability to restore at any point in time
	* 7 days retention but can be increased to 35 days
* DB Snapshots:
	* manually triggered by user
	* retention of backup as long as you want

## RDS Read Replicas
* Additional Link: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html
* helps scale reads
* can create upto 5 replicas
* Options are:
	* within AZ
	* Cross AZ
	* Cross Region
* Replication is ASYNC, meaning reads are eventually consistent. If the data is read before the replication, we may get old data.
* we can promote replicas to databases
* there's a network cost when data goes from one AZ to another
* to reduce this network cost replica should be in same AZ
* Use case:
	* read replica can be used to run queries on

## RDS Multi AZ (Disaster Recovery)
* This is a SYNC replication
* One DNS name - auto app failover to standby
* Increase availability
* Failover in case of loss of AZ/network/instance/storage
* No manual intervenion
* Not used for scaling
* Read Replicas can be setup as Multi AZ

## RDS Security
* At rest cryption
	* possible to encrypt master and read replicas with AWS KMS - AES-256 encryption
	* Has to be defined at launch time
	* if master doesn't have encryption, read replicas can't have encyption
	* Transparent Data Encryption (TDE) available for Oracle and SQL server
* In-flight encryption
	* SSL certificate
	* provide SSL option with trust certificate
	* to enforce SSL:
		* Postgres: rds.force_ssl=1 (in RDS console parameter group)
		* mysql: `grant usage on *.* to 'mysqluser'@'%' REQUIRE SSL;`
* Encryption Operations:
	* encrypting RDS backups
		* snapshots will be encrypted/unencrypted based on the database encryption
		* copy a snapshot to convert to an encrypted one
	* Steps to encrypt an unencrypted database
		1. create an unencrypted snapshot of unencrypted db
		2. copy the unencrypted snapshot and enable encryption
		3. restore the db from the encrypted snapshot
		4. migrate the application to new db and delete the old one
* Network
	* usually deployed within private subnet
	* works by using Security Groups (same as EC2 instances)
* Access Management
	* IAM policies to control who can manage RDS
	* Traditionally username and password can also be used
	* IAM based auth can be used for MySQL and PostgreSQL (auth token with life time of 15 minutes)
	* Benefits:
		* network in/out must be encrypted with SSL
		* IAM to centrally manage the users
		* can leverage IAM Roles and EC2 instances

## Amazon Aurora
* proprietory technology of AWS
* compatible wtih Postgres and MySQL
* Cloud optimized (better performance than MySQL and Postgres)
* Auto increment size increment of 10GB up to 64TB
* Can have upto 15 replicas while MySQL has 5
* Failover is instantaneous. It's HA native
* It costs more but more efficient
* Keeps 6 copies of data across 3 AZ. 4 copies out of 6 needed for writes. 3 copies out of 6 need for reads. 
* Self healing peer-to-peer replication
* Storage is striped across 100s of volumes
* One instance intakes writes (Master)
* failover happens in 30 seconds
* Master + upto 15 Aurora Read Replicas server reads
* Support Cross Region Replication
* Structure of Aurora:
	* everything is based on Shared storage volume (auto expanding)
	* One master writes to it. 
	* Master is connected through the "Writer Endpoint", which is DNS
	* The client talks to the Writer Endpoint
	* One or more Read Replicas that are on Auto Scaling
	* The client reads from the Read Replicas through "Reader Endpoint" which is a Load balancer. The load balancing happens at connection level
* Features:
	* auto failover
	* backup and recovery
	* isolation and security
	* industry compliance
	* push button scaling
	* automated patching with zero downtime
	* advanced monitoring
	* routine maintenance
	* backtrack: restore data at any point without backup
* Security
	* similar to RDS
	* encryption with KMS
	* auto backups, snapshots, and replicas are encrypted
	* encryption in flight using SSL
	* possibility of auth using IAM token
	* can't SSH
	* protect with Security Group

## Aurora Serverless
* Auto database instantiation and auto scaling based on usage
* Good for infrequent and unpredictable workloads
* no capacity planning
* pay per second
* Proxy Fleet connects client with Aurora DB

## Global Aurora
* Aurora Cross Region Replicas
	* good for DR
	* simple to put in place
* Aurora Global Database (recommended)
	* 1 primary region (r/w)
	* upto 5 secondary region (r only). less than 1 second repliation lag
	* upto 16 replicas per secondary region
	* helps to decrease latency
	* promoting another region (for DR) has Recovery Point Objective(RTO)  of less 1 minute

## ElastiCache
* to manage Redis or Memcached
* in-memory databases
* high performance, low latency
* reduce load off of db
* helps make apps stateless. session is stored in the cache
* write scaling using sharding
* read scaling using Read Replicas
* Multi AZ with failover
* AWS takes care of OS maintenance, optimization, setup, config, monitoring, failure recovery, and backups
* application queries and finds the data in cache. This is called "Cache Hit". If the data is not found in the cache, the query is forwared to RDS, which is called "Cache Miss". 

## Redis vs Memcached
* Redis
	* more like RDS
	* multi AZ with auto failover
	* Read Replicas to scale reads and high HA
	* Data Durability of AOF persistence
	* Backup and restore
* Memcache
	* Multi-node partitioning of data (sharding)
	* Non persistent
	* No backup and restore
	* multi-threaded architecture

## Caching Strategies
* Best Practices: https://aws.amazon.com/caching/best-practices/
* not for every type of data
* is it safe to cache data? may be out of date
* Is caching effective for that data? Patterns (slow changing, few keys are frequently needed), Anti-pattern(rapidly changing, large keys are frequently needed)
* Is data structured for well for caching? key-value caching or aggregate results
*  Which caching design pattern to followed?

## Caching Design Pattern
* Lazy Loading/Cache-Aside/Lazy Population: 
	* asks the cache first; cache hit or cache miss
	* write the data of cache miss to cache
	* Pros:
		* only requested data is cached
		* node failure are not fatal
	* Cons:
		* cache miss penalty result in 3 round trips of reqeusts
		* stale data in cache
* Write Through
	* add/update cache when data is updated
	* when something is written to DB also written to ElastiCache
	* Pros:
		* data in cache is never stale
		* write penalty (2 calls for write)
	* Cons:
		* missing data until it is written to DB (so Lazy loading and write through can be combined)
		* Cache churn (lot of data will never be read)
* Cache Evictions and Time-to-Live (TTL)
	* can occur in three ways:
		1. deleted explicitly in the cache
		2. memory of cache is full and not recently used (LRU - Least Recently Used)
		3. time-to-live (TTL) how much time a data can live in cache. from seconds to days
	* too many evictions should lead to reconsider scaling cache

**There are two hard things in Computer Science: Cache Invalidation and naming things**