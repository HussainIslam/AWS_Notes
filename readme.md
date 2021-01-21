<!-- MarkdownTOC -->

- EC2
	- Intro
	- Tasks
	- Security Groups
	- EC2 Launch Types
	- Elastic Network Interfaces \(ENI\)
	- EC2 Pricing
	- Custom AMI
	- EC2 Instances Peripherals
	- Notes
- ELB
	- Load Balancing
	- Why Load Balancer?
	- Why EC2 Load Balancer \(ELB\)
	- Health Checks
	- Types of ELB
	- ALB Target Groups
	- Load Balancer Stickiness
	- Cross Zone Load Balancing
	- SSL/TLS
	- Server Name Indication \(SNI\)
	- Connection Draining
	- Note
- Auto Scaling Group \(ASG\)
	- Intro
	- Scaling Policies
	- Scaling Cooldowns
- Elastic Block Store \(EBS\) and Elastic File System \(EFS\)
	- What is EBS
	- EBS volume types
	- EBS vs Instance Store
	- Elastic File System \(EFS\)
	- Note:
	- Relational Database Service \(RDS\)
	- RDS Backups
	- RDS Read Replicas
	- RDS Multi AZ \(Disaster Recovery\)
	- RDS Security
	- Amazon Aurora
	- Aurora Serverless
	- Global Aurora
	- ElastiCache
	- Redis vs Memcached
	- Caching Strategies
	- Caching Design Pattern
	- Intro
	- DNS Records TTL \(Time to Live\)
	- CNAME vs Alias
	- Routing Policies
	- Health Checks
	- 
	- # Virtual Private Cloud \(VPC\)
	- Concepts
	- Network ACL \(NACL\) and Security Group
	- VPC Flow Logs
	- VPC Peering
	- VPC Endpoints
	- Site to SiteVPN and Direct Connect
	- Three Tier Architecture
	- Intro
	- AWS CLI setup \(Local Computer\)
	- AWS CLI setup \(EC2 Instance\)
	- AWS CLI Profile
	- AWS CLI with MFA
	- AWS SDK
	- AWS Limits \(Quotas\)
	- AWS CLI Creditals Provider Chain
	- Create Policies
	- AWS CLI STS Decode Errors
	- Signing AWS API Requests
	- EC2 Instance Metadata
	- Notes
	- Intro
	- CloudFront Origins
	- CloudFront Security
	- CloudFront vs Cross Region Replicaton
	- CloudFront Caching
	- CloudFront Signed URL/Signed Cookies
	- CloudFront Signed URL vs S3 Pre-Signed URL
	- Notes
	- Intro to Docker
	- ECS Intro
	- Elastic Container Registry \(ECR\)
	- Fargate
	- ECS IAM Roles
	- ECS Tasks Placement
	- ECS - Service Auto Scaling
	- Intro
	- Beanstalk Lifecycle Policy
	- Beanstalk Extensions
	- Elastic Beanstalk Migration
	- Beanstalk with Docker
- Continuous Integration, Continuous Deployment \(CICD\)
	- Intro
	- CodeCommit
	- CodePipeline
	- CodeBuild
	- CodeDeploy
	- CodeStar
	- Intro
	- building block
	- CloudFormation Rollbacks
	- Other Concepts
	- Monitoring
	- CloudWatch
	- Amazon EventBridge
	- AWS X-Ray
	- X-Ray Instrumentation
	- CloudTrail
	- CloudTrail vs CloudWatch vs X-Ray
	- Intro
	- Amazon SQS
	- SQS Options
	- Amazon SNS
	- SNS + SQS Fan Out
	- AWS Kinesis
	- AWS Kinesis Streams
	- SNS vs SQS vs Kinesis
- AWS Lambda
	- Intro
	- AWS Lambda
	- Lambda - Synchronous Invocation
	- Lambda Integration with ALB
	- Lambda@Edge
	- Lambda - Asynchronous Invocations
	- Lambda with CloudWatch Events or EventBridge
	- Lambda with S3 Event Notification
	- Lambda - Event Source Mapping
	- Lambda Destinations
	- Lambda Execution Role \(IAM Role\)
	- Lambda Environmet Variables
	- Lambda Loggin & Monitoring
	- Lambda in VPC
	- Lambda Performance
	- Lambda Concurrency and Throttling
	- Function Dependencies
	- Lambda and CloudFormation
	- Lambda Layers
	- Lambda Versions and Alias
	- Lambda and CodeDeploy
	- Lambda Limits
	- Best Practices
	- Intro
	- Basics
	- Throughputs
	- DynamoDB APIs
	- DynamoDB CLI
	- Secondary Index
	- Optimistic Concurrency
	- DynamoDB DAX \(Accelerator\)
	- DynamoDB Streams
	- DynamoDB and Lambda
	- Additional Concepts
	- Intro
	- Canary Deployment
	- Integration
	- API Swagger/Open API spec
	- Caching API Responses
	- Usage Plans and API Keys
	- Monitoring and Logging
	- CORS
	- Security
	- HTTP API vs REST API vs WebSocket
	- Intro
	- SAM Policy Templates
	- SAM and CodeDeploy
	- Intro
	- Cognito User Pools \(CUP\)
	- Cognito Identity Pools \(federated identities\)
	- User Pool vs Identity Pool
	- Cognito Sync \(Deprecated\)
	- Step Functions
	- AppSync
	- Intro
	- IAM Federation
	- Tasks
	- Manage MFA
	- Creating Users
	- Creating Groups
	- Adding Users to Group
	- IAM Password Policy
	- Authorization Model
	- Dynamic Policies
	- Types Policies
	- AWS Directory Services
	- Notes
	- STS
	- Encryption
	- Key Management Service \(KMS\)
	- Symmetric Keys
	- KMS Key Policies
	- Envelope Encyption
	- SSM Parameter Store
	- AWS Secrets Manager
	- SSM Parameter Store vs Secrets Manager
	- CloudWatch Logs Integration
	- CodeBuild Security
	- Buckets
	- Versioning
	- Encryption
	- S3 Security
	- S3 Websites
	- Cross Origin Resource Sharing \(CORS\)
	- S3 CORS
	- S3 - Consistency Model
	- S3 MFA - Delete
	- S3 Access Logs
	- S3 Replication \(CRR & SRR\)
	- S3 Pre-signed URLs
	- S3 Storage Classes
	- Moving Between Storage classes
	- S3 Performance
	- S3 Select and Glacier Select
	- S3 Event Notifications
	- AWS Athena
	- S3 Object Lock and Glacier Vault Lock
	- Simple Email Service
	- Database Summary
	- AWS Certificate Manager \(ACM\)

<!-- /MarkdownTOC -->


# EC2

## Intro
* It can:
	* rent virtual machines (EC2)
	* Store data on virtual drives (EBS)
	* Distribute load across machines (ELB) 
	* scaling services (ASG)

## Tasks
* launch virtual server
* start/stop/terminate instances

## Security Groups
* Basically firewall on EC2 instances
* Regulate: 
	* Access to ports
	* Authorized IP ranges
	* Control inbound network
	* Control outbound network

## EC2 Launch Types
1. On Demand: short workload, predictable pricing
	* pay for what you use
	* highest cost
	* no upfront payment
	* no long term commitment
	* Use case: short term uninterrupted workloads, where application behavior can't be predicted; elastic workload
2. Reserved: Minimum 1 year
	1. Reserved Instances: long workload
		* high discount from on-demand
		* pay upfront
		* reserve for 1-3 years
		* reserve specific instance type
		* Use case: steady state usage application (like database)
	2. Convertible Reserved Instances: long workloads with flexible instances
		* can change the EC2 instance type
		* little bit more expensive
	3. Schedule Reserved instances: need some of the time (ex: every thursday from 3-6pm)
		* launch within time window
		* when require a fraction of day/week/month
3. Spot: Short workload; cheap; can lose instances; less reliable
	* high discounts
	* can lose any point in time if the max price is less than current spot price
	* the MOST cost efficient instances
	* Useful for workloads that are resilient to failure:
		* batch job
		* Data analysis
		* image processing
4. Dedicated Instances: no one else will share the hardware
	* dedicated hardware
	* may share hardware with other instances of the account
	* No control over instance placement (can move hardware after stop/start)
5. Dedicated Hosts: book entire physical server; control instance placement
	* physical dedicated EC2 server
	* full control of EC2 Instance placement
	* visibility into underlying socket/physical cores
	* 3 year reservation
	* More expensive
	* Useful for complicated licensing model (BYOL - Bring your own License)
	* Also for companies with strong regulatory requirements

* Dedicated Instances vs Dedicated Host: https://www.youtube.com/watch?v=sOsALtwltLQ
![Dedicated instance vs dedicated host](https://i.ibb.co/MS0VQnC/Screenshot-from-2020-12-14-21-32-57.png)

## Elastic Network Interfaces (ENI)
* logical component of VPC representing a virtual network card
* ENI has following attributes:
	* primary private IPv4 and one or more secondary IPv4
	* one Elastic IPv4 per private IPv4
	* One public IPv4
	* One or more security groups
	* One MAC address
	* Bound to AZ
* Additional study: https://aws.amazon.com/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/

## EC2 Pricing
* Per hour price varies based on these parameters:
	* Region
	* Instance Type
	* Linux vs Windows vs Private OS (RHEL, SLES, Windows SQL)
* Other pricing factors include:
	* storage
	* Data transfer
	* fixed public IP addresses
	* load balancing
* Billed by second with minimum of 60 seconds

## Custom AMI
* Advantages:
	* Pre-installed packages
	* Faster boot time (no need for long ec2 user data)
	* Configured monitoring/enterprise software
	* Security concern - control over machine
	* Control of maintenance and updates
	* Active Directory integration
	* Installing apps ahead of time; faster deploy when auto scaling
	* Using someone else's AMI
* AMIs are built for specific region

## EC2 Instances Peripherals
* Additional Information: https://aws.amazon.com/ec2/instance-types/
* R/C/P/G/H/X/I/F/Z/CR are good at one or more things
* M instances are balanced, not really good at something
* T2/T3 instances are burstable (generally OK but can handle sudden "burst" to handle large load and uses "burst credits")
* T2 Unlimited: Unlimited burst credit balance; need to pay extra

1. RAM (type, amount, generation)
2. CPU (type, make, frequency, generation, number of cores)
3. I/O (disk performance, EBS optimisations)
4. Network (network bandwith, network latency)
5. Graphical Processing Unit (GPU)


## Notes
* Security groups can be attached to multiple instances
* Security groups are specific to region-VPC combination
* Security groups lives outside EC2
* Good to maintain separate security group for SSH
* By default, all inbound are blocked and all outbound are allowed
* A security group can be used in other security groups
* machines connect to internet through NAT + internet gateway (a proxy)
* One can only have 5 elastic IPs in an account but can request AWS to increase it
* Better to use random public ip and attach a DNS to it rather than using public ip


# ELB

## Load Balancing
Load Balancers are servers that forward internet traffic to multiple servers (EC2 instances) downstream. Sits between the users requesting and the servers. The Load Balancer gets the requests and forwards those to respective servers and vice versa.

## Why Load Balancer?
* Spread load across multiple downstream instances
* Expose single point of access (DNS)
* Handle failures of downstream instances
* Do regular health checks to instances
* Provide SSL termination (HTTPS) for websites
* Stickiness with cookies
* High Availability across zones
* seperate public and private traffic

## Why EC2 Load Balancer (ELB)
* Managed load balancer
* AWS guarantee
* AWS takes care of updates/maintenance/availability
* AWS provides configuration knobs
* less effort but high cost to setup
* integrated with many AWS services

## Health Checks
* cruicital for ELB
* ELB can know whether a server is healthy
* done on a port and a route ("/health" is common)
* Response is 200(OK) for healthy instance

## Types of ELB
There are 3 types:
1. Classic Load Balancer (CLB): 
	* v1 - old generation
	* 2009
	* Support: HTTP, HTTPS, TCP
	* Health check on TCP/HTTP
	* Fixed hostname: xxx.region.elb.amazonaws.com
2. Application Load Balancer (ALB):
	* v2 - new generation
	* 2016
	* Supports: HTTP, HTTPS, WebSockets
	* Multiple HTTP applications accross machines (target groups)
	* load balancing to multiple applications on the same machine (containers)
	* supports HTTP2 and WebSocket
	* Support redirects (from HTTP to HTTPS)
	* has port mapping feature to redirect to a dynamic port in ECS
	* Routing tables to different target groups:
		* based on URL (/user and /posts)
		* based on hostname in url (one.abc.com and two.abc.com)
		* based on query string, headers
	* good fit for microservices and container based applications
	* health checks are target group level
3. Network Load Balancer (NLB):
	* v3 - New generation
	* 2017
	* Supports: TCP, TLS (secure TCP) and UDP
	* Lower level in OSI Framework (layer 4)
	* Handle millions of requests per seconds
	* Less latency ~ 100ms
	* Expose one static IP per AZ and supports assigning Elastic IP (good for whitelisting specific IP)
	* Good for extreme performance
	* NOT IN FREE TIER


Based on where ELB is setup, it is of two types:
1. Internal/Private (can't be acessed from outside)
2. External/Public (can be acessed from outside)

## ALB Target Groups
1. EC2 instances
2. ECS tasks
3. Lambda functions
4. IP addresses

## Load Balancer Stickiness
* possible to implement stickiness so that the same client is always redirected to the same instance
* applicable for CLB and ALB
* "cookie" is used for the stickiness, which has exiry date
* to make sure user doesn't lose data
* may add some imbalance to the load

## Cross Zone Load Balancing
* distributes load evenly across all registered instance in all AZ
* otherwise the LBs will divert only within AZ

## SSL/TLS
* SSL certificate encrypts data between client and LB (this is called in-flight encryption)
* SSL mean Secure Sockets Layers (older version)
* TLS mean Transport Layer Security (newer version)
* TLS are mostly used but still referred to as SSL
* Public SSL certificates by Certificate Autorities (CA): Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt
* LB does "SSL Termination" after receiving a HTTPS request and forwards it to instance over HTTP
* LB loads x.509 certificates, which can be managed by ACM (AWS Certificte Manager)
* Must specify a default certificate and add optional list of certificates to support multiple domains.
* Clients use SNI (Server Name Indication) to specify the hostname they reach

## Server Name Indication (SNI)
* Solves the problem of loading multiple SSL certificates to one webserver to server multiple websites
* it's a newer protocol that require the client to indicate the hostname of the target server in the initial SSL handshake. The sever will then find the correct certificate or return the default one
* Works with ALB, NLB and CloudFront

## Connection Draining
* In CLB it's called "Connection Draining"
* For Target Group (ALB and NLB) it's called "Deregistration Delay"
* Time to complete in-flight requests while the instance is deregistering or unhealthy
* LB stops sending new requests to the instance that is deregistering
* Default is 300 seconds but can be from 1 to 3600 seconds. Can be disabled by setting it to zero
* Should be set based on how long the instance usually takes to respond to a request

## Note
* ELB would need security group setup for connection from anywhere. However, the instance security group should receive information only from the ELB
* ELBs can scale but not instantaneous - contact AWS for "warm-up"
* Troubleshooting:
	* 4xx: client induced error
	* 5xx: application induced erros
	* 503: at capacity or not registered target
	* if ELB can't connect, need to check security group
* Monitoring:
	* ELB access logs will log all access requests
	* CloudWatch Metrics will give aggregate statistics
* For NLB, the instances see the traffic coming from the internet. So, we need to update the security group of the instance to allow TCP connection on port 80 from anywhere!!
* Cross Zone Load Balancing is disabled by default for CLB and will not be charged; for ALB its always enabled and there are not additional charges; on NLB its disabled by default and there will be additional charges
* CLB doesn't SNI but ALB and NLB does


# Auto Scaling Group (ASG)

## Intro
* loads on websites usually change
* goal ASG is to
	* scale out (add EC2 instances)
	* scale in (remove EC2 instances)
	* ensure we have minimum and maximum number of machines running
	* auto register instances to load balancer
* has the following attributes:
	* launch configration
		* AMI + instance type
		* EC2 User Data
		* EBS Volumes
		* Security Groups
		* SSH Key Pair
	* Min/Max size and Initial Capacity
	* Network and Subnet information
	* Load Balancer information
	* Scaling policies
* Scaling can be based on CloudWatch alarms, which monitor some metrics
* alarm rules can be:
	* target average cpu usage
	* number of requests on ELB per instance
	* average network in
	* average network out
* Auto scaling can be based on custome metrics
* ASGs use Launch Configuration or Launch Templates (newer)
* To update ASG, need to provide new launch config/template
* IAM roles attached to ASG will get assigned to EC2 instances
* ASG free but instances are not
* if instances get terminated, ASG will auto create another one
* ASG can terminate instance marked "unhealthy" by LB

## Scaling Policies
* Target Tracking Scaling
	* most simple
	* Example: average ASG cpu to be 40%
* Simple/Step Scaling
	* CloudWatch alarm triggered (CPU>70%), add 2 add units
	* CloudWatch alarm triggered (CPU<30%), remove 1 unit
* Scheduled Actions
	* Anticipate scaling based on know usage patterns
	* increase min capacity to 10 at 5pm on friday

## Scaling Cooldowns
* cooldown period helps to prevent ASG from launching/terminating instances befoe previous scaling activity takes effect
* we can create cooldowns applicable for Simple Scaling Policy
* Scaling specific cooldown period overrides default
* If default 300 seconds is too long - can be reduced to 180 seconds to reduce cost
* If application is scaling up and down multiple times each how, modify the Auto Scaling Groups cooldown timer and the CloudWatch Alarm Period that triggers the scale in


# Elastic Block Store (EBS) and Elastic File System (EFS)

## What is EBS
* EBS volume is a network drive we can attach to our instance
* EC2 will lose root volume once terminated. To avoid this we can attach EBS to it.
* Persists data
* It's a network drive, which means there may be latency
* Being a network drive, it can be detached from one EC2 instance and attached to another
* Locked to AZ
* Need to have provision capacity (GB/IOPS)
* Billed for provision not for use
* Capacity can be increased over time without losing data

## EBS volume types
* Additional links: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html
* Volumes are characterized by:
	* Size
	* Throughput
	* IOPS (I/O operations per second)
* Types:

1. GP2 (SSD): General purpose SSD; balances price and performance
	* Use cases:
		* recommended for most workloads
		* system boot volumes
		* virtual desktops
		* low latency interactive apps
		* development and test environment
	* small gp2 volume can "burst" IOPS to 3000
	* Max IOPS possible is 16000, minimum 100
	* EBS allows 3 IOPS per GB; hence, max size is 5334GB for IOPS

2. IO1(SSD): high performance SSD; mission critical, low latency or high throughput workloads
	* Use cases:
		* sustained IOPS performance, or more than 16000 IOPS
		* large database workload
	* 4GB - 16TB
	* IOPS is provisioned (PIOPS)
	* minimum IOPS is 100
	* Maximum IOPS for general instances: 32000, for Nitro Instances: 64000
	* Maximum ratio of PIOPS to volume size is 50:1
3. ST1(HDD): low cost, designed for frequent access, throughput intensive workloads
	* Use cases:
		* streaming workloads requiring consistent and fast throughput at low price
		* bid data, data warehouse, log processing
		* Apache Kafka
		* Can't be boot volume
	* 500GB - 16 TB
	* Max IOPS is 500
	* Max throughput 500MB/s (can burst)
4. SC1(HDD: Lowest cost, less frequently accessed workload
	* Cold HDD
	* Use cases:
		* throughput oriented storage with infrequent access
		* when lowest cost storage is needed
		* can't be boot volume
	* 500GB - 16TB
	* max IOPS of 250
	* Max throughput of 250MB/s

## EBS vs Instance Store
* Some instances don't have Root EBS volumes rather have "Instance Store", which are ephemeral storage
* Instance stores are physically attached to the machine
* Disks upto 7.5 TiB and stipped to reach 30 TiB
* Block storage (like EBS)
* Size is fixed after creating
* Risk of data loss in case of hardware failure
* Pros:
	* better I/O performance. very high IOPS
	* Good buffer/cache/scratchdata
	* Data servives reboots
* Cons
	* on termination, all data is lost
	* can't resize on the fly
	* backups must be manually done

## Elastic File System (EFS)
* Manageed NFS (Network File System) that can be mounted on may EC2
* EFS works with EC2 instances in multi AZ
* highly available, scalable, expensive
* Pay per use
* Security gorup need to be attached
* Use case:
	* content management
	* web serving
	* data sharing
	* wordpress
* Uses NFSv4.1 protocol
* only for Linux AMI (not windows)
* Encryption at rest using KMS
* POSIX file system that has standard API
* Scale automatically
* Pros:
	* It can handle 1000s of NFS clients
	* 10GB+ per second throughput
	* Grow to petabyte scale network file system automatically
* Performance Modes
	* General Purpose (Default): latency sensitive use cases (web server, cms)
	* Max I/O: high latency, throughput, high parallel (big data, media processing)
* Storage Tiers (Lifecycle management features - move files after "N" days)
	* Standard: for frequently accessed files
	* Infrequently access (EFS-IA): lower price to store, incur retrival fee to read files
* Mounting EFS: https://docs.aws.amazon.com/efs/latest/ug/mounting-fs.html


## Note:
* Only GP2 and IO1 can be boot volume
* Preparing EBS for use: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html # Relational Database Service (RDS)

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

**There are two hard things in Computer Science: Cache Invalidation and naming things**# Route 53

## Intro
* manage DNS (Domain Name System)
* DNS is a collection of rules and records that helps clients understand how to reach a server through its domain name
* Most common records are:
	* A: hostname to IPv4
	* AAAA: hostname to IPv6
	* CNAME: hostname to hostname
	* Alias: hostname to AWS resource
* Route53 can use:
	* public domain name: application.mypublicdomain.com
	* private domaain names that can be resolved by your instances in VPC: application.company.internal
* Advanced features:
	* load balancing (called Client Load Balancing)
	* Health checks
	* routing policy: simple, failover, geolocation, latency, weighted multi value
* payment of $0.5 per month per hosted zone

## DNS Records TTL (Time to Live)
* When a client requests a DNS, Route53 sends back the IP and TTL. TTL is the amount of time the browser will cache the IP address response from Route53.
* High TTL means less traffic on DNS but outdated records
* Low TTL means high traffic on DNS, less chances of outdated records.
* TTL is mandatory for each DNS records

## CNAME vs Alias
* AWS resources (Load Balancer, CloudFront) expose an AWS hostname and we would like to point our hostname to that.
* CNAME points a hostname to other hostname but that can't be a root hostname. For example: something.abc.com can be routed using CNAME but not abc.com
* Alias points a hostname to an AWS resource. Example: someting.abc.com can point to anything.amazonaws.com. This also works for root and non-root domains. It is also free of charge and have native health check.

## Routing Policies
* Simple Routing Policy: 
	* redirect to a single resource
	* can't attach health check
	* if multiple values are returned, a random one is chosen by client (we can add multiple DNS targets in the record)
* Weighted Routing Policy:
	* control percentage of requests that go to a specific endpoint
	* For example: 20% to one EC2 instance, 80% to another EC2 instance
	* helpful to test 1% of traffic on new app version
	* Helpfult to split traffic between two regions
	* can be associated with health checks
	* will need to create multiple records specific weight for each
* Latency Routing Policy:
	* redirect to server that has least latency close to client
	* super important when latenc of users is important
	* latency is evaluated in terms of user to designated AWS region
	* will need to create multiple records for required instances
* Failover Routing Policy
	* There needs to be at least 2 targets. One is primary and the other one secondary (disaster recovery). In case of a failure the secondary one will be activated. That is all the DNS requests will be routed to secondary. For this health check on the primary is mandatory
	* We need to create two policy (one for primary and one for secondary) with routing policy as "Failover"
* Geolocation Routing policy:
	* different from Latency!
	* based on user location
	* traffic from UK to go to a specific IP
	* should create a default policy, in case there's no match
* Multivalue Routing Policy:
	* routing to multiple resources
	* associate a Route53 health check with records
	* up to 8 healthy records are returned for each query
	* Multi value is not substitute for having ELB

## Health Checks
* This is similar to Load Balancer
* if an instance is not healthy, Route53 will not send traffic to that.
* By default 3 consecutive health check fail would mean unhealthy
* Default interval is 30 seconds but can be set to 10 seconds with higher cost
* About 15 health check the endpoint health. So that's about one request every 2 mintutes
* Can have HTTP, TCP, and HTTPS check (no SSL verification)
* can be linked to Route53 DNS queries

## 

## # Virtual Private Cloud (VPC)

## Concepts

* private network to deploy your resources
* regional resource
* vpc is logical contruct
* subnets allow to partition network inside VPC
* Public Subnet is accessible from internet
* Private Subnet is not accessible from internet
* To define access to internet and between subnets, we use Route Tables
* Internet Gateways (IGW) helps our VPC to connet to internet
* Public Subnets have a route to IGW
* NAT Gateways (AWS Managed) and NAT Instances (self managed) allow your instance in private subnets to access internet while remaining private. Add a NAT in public subnet and then create a route from private subnet to NAT and then the NAT to IGW

## Network ACL (NACL) and Security Group
* NACL is like a firewall that controls traffic from and to subnet. It can have ALLOW and DENY rules. These are attached to Subnet leve. Rules only include IP address. Default is allow all incoming and outgoing traffic.
* Security Groups control traffic to and from an ENI/EC2 instance. This can only have ALLOW rules. Rules include IP addresses and other security groups. Stands between NACL and ENI/EC2.

## VPC Flow Logs
* this captures information about IP traffic going into interfaces:
	1. VPC Flow Logs
	2. Subnet Flow Logs
	3. Elastic Network Interface Flow Logs
* Help monitor and troubleshoot connectivity issues
	* subnet to subnet
	* subnet to internet
	* internet to subnet
* Captures network info from AWS managed interfaces: ELB, ElastiCache, RDS, Aurora
* VPC Flow logs data can go to S3/CloudWatch Logs

## VPC Peering
* connects two VPCs privately using AWS network
* make them behave as if the were in the same network
* the IP ranges (CIDR) of these VPCs must not overlap
* VPC Peering is not transitive. That is must be established for each VPC that need to be connected
* 

## VPC Endpoints
* allows to connect to AWS sevices using private network. Because all AWS services are public. 
* give enhanced security and lower latency
* VPC Endpoint Gateway: S3/DynamoDB
* VPC Endpoint Interface: Rest of the things
* Only used within VPC

## Site to SiteVPN and Direct Connect
* Site to SiteVPN
	* connect an on premise VPN to AWS
	* connection is encrypted
	* goes over public internet
* Direct Connect (DX)
	* physical connection between on-premise and AWS
	* This is a physical connect
	* private, secure, and fast
	* goes over private network
	* takes at least a month to establish
* Site-to-Site VPN and Direct Connect can't access VPC Endpoints

## Three Tier Architecture
* Public Subnet: Includes Route53 + ELB
* Private SUbnet: Auto Scaling Group + EC2
* Data Subnet: RDS, ElatiCache# AWS CLI

## Intro
* Developing and performing AWS tasks can be done in:
	* AWS CLI on local computer
	* AWS CLI on EC2 machines
	* AWS SDK on local computer
	* AWS SDK on EC2 machines
	* AWS Instance Metadata Service on EC2

## AWS CLI setup (Local Computer)
* Find the user Security Credentials on IAM under 'Security Credentials' section of 'User'. 
* Create a new "Access Key"
* on Terminal of your computer run `aws configure`
* Paste in the ID of the access key and press enter
* Paste in the Access Key and press enter
* Enter the name of the AZ
* Enter format (default is JSON)

## AWS CLI setup (EC2 Instance)
**Must be done with IAM Roles**
* ssh into the EC2 instance
* `aws configure` the instance WITHOUT any ID and access key
* Go to IAM > Roles > Create Role > AWS Service
* Select EC2 and click Next
* Select a Policy: example: AmazonS3ReadOnlyAccess. Click Next > Next
* Enter Name and Description and click Create Role
* Go to EC2 and right click on the the Instance
* Security > Modify IAM Role
* Select the created IAM Role and Save
* Now in our terminal, if we type `aws s3 ls` we should be able to see results

## AWS CLI Profile
* creating profile: `aws configure --profile <profile-name>`
* run a command with a profile: `aws s3 ls --profile <profile-name>`

## AWS CLI with MFA
* must create a temporary session which can be done by running `STS GetSessionToken` API call
* `aws sts get-session-token --serial-number <arn-of-mfa-device> --token-code <code-from-token> --duration-seconds <number-of-seconds>`
* ARN of MFA device can be found in IAM User (if MFA device is not setup, will need to create a MFA device first)
* This will return token details, which need to be added to the `~/.aws/credentials` file using a text editor.
* Sample:
```
[mfa]
aws_access_key_id = <string>
aws_secret_access_key = <string>
aws_session_toke = <string>
```

## AWS SDK
* used to perform actions on AWS from applications
* Official SDK supports:
	* Java
	* .NET
	* Python (boto3/botocore)
	* Node.js
	* C++
* `us-east-1` is default region

## AWS Limits (Quotas)
* API Rate Limit
	* DescribeInstances API for EC2 has 100 call/second
	* GetObject on S3 has 5500 GET/second per prefix
	* If go over limit (intermittent): implement Exponential Backoff
	* If go over limit (consistent): request throttling limit increase
* Service Quotas/Limits:
	* we can run 1152 vCPU of on-demand Standard instances
	* can request service limit increase by opening a ticket
	* can request service quota increase by using service quotas API
* Exponential Backoff
	* ThrottlingException once in a while
	* This is based on "Retry" mechanism
	* Must implement manually using API as is or specific cases
	* basically doubles the time before requesting

## AWS CLI Creditals Provider Chain
* CLI will look for credentials in the following order:
	1. command line options (region/output/profile)
	2. Environment variables (AWS_ACCESS_KEY_ID and etc)
	3. CLI Credential file (`~/.aws/credentials`)
	4. CLI configuration file (`~/.aws/config`)
	5. Container credentials (for ECS tasks)
	6. Instance profile credentials (for EC2 Instance Profiles)
* AWS SKD order:
	1. Environment Variables
	2. Java System Properties
	3. Default credential profiles file
	4. Amazon ECS Container Credentials (ECS containers)
	5. Instance profile credentials (EC2 instances)

## Create Policies
* Can be created using "Visual Editor" or typing in JSON. We can also use the policy generator.
* We can also create an inline policy that can be used only with one role
* To check policies through AWS CLI we can add option `--dry-run`

## AWS CLI STS Decode Errors
* API calls might give long error messages, which can be decoded using the STS command line.
* We would need to add this permission to the policy to decode the messages
* `aws sts decode-authorization-message --encoded-message <message>`
* referece: https://docs.aws.amazon.com/cli/latest/reference/sts/decode-authorization-message.html

## Signing AWS API Requests
* should sign AWS HTTP requests using Signature v4 or SigV4

## EC2 Instance Metadata
* EC2 instances can learn about themselves
* URL is `http://169.254.169.254/lates/meta-data`
* this is an internal url and will not work from outside. So, we can SSH into and EC2 Instance and run `curl http://169.254.169.254/latest/meta-data/hostname`
* Metadata is info about the EC2 Instance

## Notes
* AWS CLI Reference: https://docs.aws.amazon.com/cli/latest/reference/
* IAM Policy Simulator Guideline: https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html
* IAM Policy Similator: https://policysim.aws.amazon.com/home/index.jsp?## AWS CloudFront

## Intro
* Content Delivery Network (CDN)
* Improves read performance as content is cached at Edge level
* Give DDoS protection, integration with Shield, AWS Web Application Firewall
* can expose external HTTPS and can talk to internal HTTPS backends

## CloudFront Origins
* S3 Bucket
	* for distributing files and cachin them at the edge
	* enhanced security with CloudFront Origin Access Identity (OAI), which allows S3 bucket to communicate only from CloudFront and no where else
	* can use it as ingress to upload files to S3
* Custom Origin (HTTP)
	* ALB
	* EC2 Instance
	* S3 Website (must first enable bucket as static website) 
	* Any HTTP backend

## CloudFront Security
* Geo Restriction
	* can whitelist or blacklist users from a region
	* uses 3rd party Geo-IP database
* HTTPS
	* Viewer Protocl Policy:
		* redirect HTTP to HTTPS
		* use HTTPS only
	* Origin Protocol Policy (HTTP or S3):
		* HTTPS only
		* Match viewer (HTTP to HTTP and HTTPS to HTTPS)
	* S3 Bucket websites does't support HTTPS
 
## CloudFront vs Cross Region Replicaton
* CloudFront:
	* global edge network
	* files are cached for a TTL
	* great for static content
	* contents can be little old
* S3 CRR: 
	* must be setup for each region
	* files are replicated in real-time
	* read only
	* great for dynamic content that needs to be available at low-latency in few regions

## CloudFront Caching
* Cache based on:
	* headers
	* session cookies
	* query string paramters
* it lives at each CloudFront Edge Location
* maximize cache hit and minimize request on origin
* TTL can be set by origin using Cache-Control header, Expires header
* can invalidate part of the cache using CreateInvalidation API
* better to direct static content to S3 Bucket and direct dynamic content (REST and HTTP server) to ALB + EC2 instances

## CloudFront Signed URL/Signed Cookies
* manage who can view the contents
* need to attach a policy:
	* includes URL expiration
	* IP ranges to access the data
	* Trusted signers (who in AWS can create these URLs)
* How long the URL will be valid for:
	* Shared Content: short period of time
	* Private Content: may last for years
* Signed URL: for individual file
* Signed Cookie: for multiple files

## CloudFront Signed URL vs S3 Pre-Signed URL
* CloudFront Signed URL:
	* allow access to path, no matter the origin
	* account wide key-pair, only root can manage
	* can filter by IP, path, date, expiration
	* can leverage caching
* S3 Pre-signed URL
	* issue a request as a person who pre-signed the url
	* uses IAM key to sign
	* limited lifetime

## Notes
* DNS Propagation: https://stackoverflow.com/questions/38735306/aws-cloudfront-redirecting-to-s3-bucket# Elastic Container Registry (ECS)

## Intro to Docker
* software development platform to deploy apps
* apps are packaged in containers and can be run on any OS
* apps run the same, regardless of OS, machine, versions
* images are stored in Docker repository
	* public repository: Docker Hub.
	* private repository: Amazon ECR (Elastic Container Registry)
* Docker Containers Management platforms:
	* ECS: Amazon's platform
	* Fargate: Amazon's serverless platform
	* EKS: Amazon's Kubernetes (open source)

## ECS Intro
* ECS Clusters:
	* logical grouping of EC2 Instances
	* EC2 Instances run ECS Agent (Docker conatiner)
	* ECS agenst registers the instance to ECS Cluster
	* these are not plain EC2 instances
* ECS Task Definition
	* metadata on how to run Docker Container
	* are in JSON format
	* Contains:
		* image name
		* port binding for container and host
		* memory and CPU requirements
		* environment variables
		* network information
* ECS Task
	* this is a running container with the settings defined in Task Definition
	* similar to "instance" of Task Definition
* ECS Service
	* help define how many tasks should run and how they should run
	* ensure that the number of tasks desired is running across our fleet of EC2 instances
	* it can be run along with ELB/NLB/ALB as well with Dynamic Port Forwarding
	* created at Cluster level
* **Containers in Task; defined by Task Definition and run in EC2 Instances; aggregated and managed by ECS Services**

## Elastic Container Registry (ECR)
* it is a private Docker image repository
* Access is controlled through IAM (permission errors => policy)
* AWS CLI v1 login command: `$(aws ecr get-login --no-include-email --region eu-west-1)`
* AWS CLI v2 login command: `aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 1234567890.dkr.ecr.eu-west-1.amazonaws.com`
* Docker push command: `docker push 123456.dkr.ecr.eu-west-1.amazonaws.com/demo:latest`
* Docker pull command: `docker pull 123456.dkr.ecr.eu-west-1.amazonaws.com/demo:latest`

## Fargate
* AWS manages the infrastructure
* it's serverless
* don't need to provision EC2 Instances
* We create Task Definisions and rest of the things are managed by AWS

## ECS IAM Roles
* EC2 Instance Profile:
	* EC2 Instance runs ECS Agent. To run it, we need to attach an EC2 Instance Profile at EC2 Instance level. 
	* This profile is used by ECS Agent to make API calls to ECS service.
	* This provile also used to send container logs to CloudWatch Logs
	* also used to pull Docker images from ECR
* ECS Task Role:
	* Instances also run Tasks
	* We can attach Task Roles to tasks
	* Should used different task roles for different ECS Services
	* Task role is defined in Task Definition

## ECS Tasks Placement
* when EC2 type task is launched, ECS must determine where to place it based on CPU, memory, and available port
* similar is true for Scale-in (removing container)
* we can define Task Placement Strategy and Task placement constraint
* Task Placement Strategies are best effort
* Procss:
	1. identify instances that meet CPU, memory and port requirements
	2. identify instances that meet Task Placement Constraints
	3. Identify instances that meet Task Placement Strategy
* ECS Task Placement Strategies
	* Binpack: place tasks based on lease availale amount of CPU or memory. Fills one instance first before using another. most cost saving
	* Random: Places tasks randomly
	* Spread: places tasks evenly based on specified value (example AZs)
	* Mixed: spread on AZ + spread on instance; spread on AZ + binpack on memory
* ECS Task Placement Constraints
	* distinctInstance: places each task on different container instance
	* memberOf: places task on instances that satisfy an expression; defined using Cluster Query Language

## ECS - Service Auto Scaling
* CPU and RAM is tracked in CloudWatch at ECS Service level
* Target Tracking: target a specific metric (example: CPU transaction at 60%)
* Step Scaling: scale based on CloudWatch alarms
* Scheduled Scaling: based on predictable changes
* ECS Service Scaling (task level) is NOT same as EC2 Auto Scaling (instance level)
* Cluster Capacity Provider: 
	* To use both ECS Service Scaling and EC2 Auto Scaling, 
	* used in association with cluster to determine the infrastructure that a task runs on# Elastic Beanstalk

## Intro
* Helps to produce the 3-tier architecture (Public, Private, and Data Subnet)
* developer centric view of deploying applicatioins on AWS
* itself is free but pay for underlying infrastructure
* it is a managed service
	* Instance config
	* deployment strategy
* Architecture model:
	1. Single Instance Deployment: good for dev
	2. LB + ASG: good for web app in prod/pre-prod
	3. ASG (worker tier): non-web app in prod;
* Components:
	1. Application
	2. Application version: each deployment gets a new version
	3. Environment name: dev/test/prod
* deploy app versions to environments and versions can be promoted to next environment
* can also do rollback to previous environment
* Deployement Modes:
	1. Single Instance: for dev
	2. High Availability w/ or w/o LB: for prod
* Deployment Options for Updates:
	* All at once(deploy all in one go): fastest but will not be available for a bit 
	* Rolling: few instances at a time (buckets)
	* Rolling with additional batches: spins new instances to move the batch so that old apps are available
	* Immutable: spins new instance in new ASG, deploys versions to these instances, and finally swaps instances when everything is healthy
* Blue/Green: 
	* NOT directly from beanstalk
	* zero downtime
	* create new stage environment and deploy v2 there
	* the new environment (green) can be validated independently and rolled back if needed
	* Route53 can be setup using weighted policies
	* using beanstalk, swap URLs when done
* Additional: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html
* Beanstalk Sample project: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html
* EB CLI: additional CLI to use Beanstalk. Basic commands include: `eb create`, `eb status`, `eb health`, `eb events`, `eb logs`, `eb open`, `eb deploy`, `eb config`, `eb terminate`
* Elastic Beanstalk uses CloudFormation under the hood, which is used to provision AWS services
* Beanstalk environment can be cloned
* HTTPS with Beanstalk:
	1. load SSL certificate into LB through console/code (`.ebextensions/securelistener-alb.config`)
	2. SSL certificate can be provisioned through ACM
	3. Allow security group rule
* Web server vs Worker environment
	* douple long running tasks from actual app
	* define tasks in cron.yaml

## Beanstalk Lifecycle Policy
* beanstalk can store upto 1000 versions
* to deploy more, will need to remove some
* Lifecycle policy can used to phase out some versions:
	* Based on time
	* Based on space
* currently used ones will not be deleted

## Beanstalk Extensions
* all the parameters set in Beanstalk console can be configured with code using files
* add `.ebextensions/` directory in the root of source code
* in YAML/JSON format
* extsion of file should be `.config`
* modify some default settings using: `option_settings`
* RDS, ElastiCache, DynamoDB can be added
* resources get deleted if environment is remove

## Elastic Beanstalk Migration
* Load Balancer:
	* after creating Beanstalk environment, ELB type can't be changed but migrate
	* Steps:
		1. Create new environment without LB
		2. Change the LB type
		3. do CNAME swap or Route53 update
* Decouple RDS:
	1. create a snapshot of RDS DB
	2. go to RDS console and protect the RDS from deletion
	3. create a new Elastic Beanstalk environment without RDS
	4. perform CNAME swap or Route 53 update
	5. Terminate old environment (RDS will not be deleted because of deletion protection)
	6. Delete CloudFormation stack because it will be in DELETE_FAILED state

## Beanstalk with Docker
* Single Docker:
	* run app as single docker container
	* provide either:
		* Dockerfile
		* Dockerrun.aws.json
	* Doesn't use ECS
* Multi-Docker:
	* Multiple containers per EC2
	* will create:
		* ECS Cluster
		* EC2 Instances for ECS
		* Load Balancer (High Availability)
		* Task Definition
	* Create `Dockerrun.aws.json` at root directory
	* Docker Images must be pre-built

# Continuous Integration, Continuous Deployment (CICD)

## Intro
* We would like to push our code in a repository and have it deployed on AWS:
	* automatically
	* the right way
	* tested before deploying
	* with different stages (dev/test/preprod/prod)
	* manual approval, if neeeded
* Parts:
	* CodeCommit: store code
	* CodePipeline: automating pipeline from code to ElasticBeanstalk
	* CodeBuild: building and testin code
	* CodeDeploy: deploying to EC2 fleet
* Continuous Integration:
	* devs push code to repository often (github/codecommit/bitbucket)
	* testing/build server as soon as it's pushed (codeBuild/Jenkins CI)
	* devs get feedback from build server
	* find bugs early, fix bugs
	* deliver faster as code is tested
	* deploy often
* Continuous Delivery:
	* ensure software can be released reliably
	* ensure deployments happen often and quick
	* shift away from "one release every 3 months"
	* Options: CodeDeploy, Jenkins CD, Spinnaker
* Technology Stack for CICD
	1. Code: CodeCommit/GitHub
	2. Build: CodeBuild, Jenkins CI, 
	3. Test: CodeBuild, Jenkins CI, 
	4. Deploy: Beanstalk, CodeDeploy
	5. Provision: Beanstalk, CloudFormation
	6. Orchestration: CodePipeline

## CodeCommit
* version control
* lets collaborate
* make sure the code is backed-up
* traits:
	* private Git repositories
	* no size limit
	* fully managed, highly available
	* increased security and compliance
	* secure (encryption, access control)
	* integrated with other 3rd party tools
* Authentication:
	* SSH Keys: configured in IAM Console
	* HTTPS: AWS CLI Authentication Helper or Generating HTTPS Credentials
	* MFA: extra safety
* Authorization: IAM Policies users/roles
* Encryption:
	* Encryption at-rest: KMS
	* Encryption in-transit: SSH/HTTPS
* Cross Account Access:
	* AWS STS (with AssumeRole API)
* It can trigger notifications using 
	* SNS (Simple Notification Service)
	* AWS Lambda 
	* AWS CloudWatch Event Rules
* Use cases of SNS/Lambda:
	* Deletation of branches
	* pushes in master branch
	* notify external build system
	* AWS Lambda to perform codebase analysis (credential got committed)
* Use cases of CloudWatch Event Rules:
	* Trigger of pull requests(create/update/delete/commented)
	* Commit comment events
	* goes into SNS topic

## CodePipeline
* For continuous delivery
* visual workflow
* Source: github/codecommit/S3
* build: codeBuild/Jenkins
* Load testing: 3rd party tools
* Deploy: CodeDeploy/BeanStalk/CloudFromation/ECS
* Made of Stages: sequential/parallel actions; build/test/deploy/load test; manual approval can be added
* must have an IAM role attached to it
* each stage can create artifacts; can be stored in S3 and passed to the next stage
* state changes generate CloudWatch Events, which in turn generate SNS notifications; 
	* create event for failed pipeline
	* create event for cancelled stages
* API Calls to it can be audited using CloudTrail
* If pipeline can't perform an action, make sure the permission of IAM Service Role
* as stage can have multipe action groups (sqeuantial or parallel)

## CodeBuild
* full managed build service
* alternative to Jenkins
* continuous scaling (no servers to manage; no build queue)
* pay for usage
* leverages docker under the hood
* can add capabilities using own base Docker images
* security: 
	* integratin with KMS for encryption of build artifacts
	* IAM for build permissions
	* VPC for network security
	* CloudTrail for API call logging
* source code from gitHub/CodeCommit/CodePipeline/S3
* Build instructions defined by `buildspec.yml` file
	* at the root of code
	* define environment variables:
		* plaintext variables
		* security secrets (SSM Parameters)
	* Phases:
		* Install dependencies
		* Prebuild: commands to execute for build
		* Build
		* Post build: finishing touch
	* At the end get Artifacts (KMS Encryption as well)
	* Cache: Files to cache; usually intermediate artifacts
* Outputs log to S3 and CloudWatch Logs
* metrics to monitor metrics
* cloudwatch events or AWS Lambda as a glue SNS Notification
* can be reproduced locally to debug (CodeBuild Agent)
* can be defined within CodePipeline or CodeBuild
* By default CodeBuild containers are launched outside VPC. To access need to specify:
	* VPC ID
	* Subnet ID
	* Security Group ID

## CodeDeploy
* helps to deploy app to many EC2 instances but not managed by Beanstalk
* Several ways to handle deployment: Ansible, Terraform, Chef, Puppet
* Managed service
* Steps:
	* EC2/Local machine must run CodeDeploy Agent
	* Agent will continuously poll CodeDeploy to work
	* CodeDeploy will send appspec.yml (at the root of project)
	* App is pulled from GitHub/S3
	* EC2 run the deployment instructions
	* CodeDeploy Agent will report success/failure
* EC2 Instances are grouped by deployment group (dev/test/prod) but have flexibility
* can be integrated with CodePipeline and use artifact
* can use existing setup tools, works with any app, auto scaling integration
* Can do Blue/Green deployment (not on premise)
* support AWS Lambda
* only deploys; doesn't provision resources
* Primary Concepts:
	* Application: unique name
	* Compute platform: EC2/on-premise
	* Deployment configuration: Deployment rules for success/failure
	* Deployment groups: group tagged instances (allow to deploy gradually)
	* Deployment type: In-place or Blue/Green
	* IAM Profile: required to pull code from S3/Github
	* Application: Code + appspec.yml file
	* Service role: role fo CodeDeploy to perform actions
	* Target revision: target version of upgrade of app
* appspec.yml:
	* File section: how to source/copy code
	* Hooks: what to do to deploy code (can have timeouts)
		* ApplicationStop: Stops the current version of app
		* DownloadBundle: download the new app
		* BeforeInstall: prep jobs before install
		* Install
		* AfterInstall: clean up after install
		* ApplicationStat: how to start app
		* ValidateService: validate the application is working
		* BeforeAllowTraffic
		* AllowTraffic
		* AfterAllowTraffic
	* Configs:
		* One at a time
		* Half at a time
		* All at once
		* Custom
	* Failures:
		* instances stay at failed state
		* new deployments will be deployed to failed states first
		* Rollback: redploy or enable automated rollback
	* Deployment targets:
		* set of EC2 instances with tags
		* Directly to ASG
			* can have in-place or Blue/Green (need ALB)
		* mix of Tags and ASG
		* Customization in scripst with DEPLOYMENT_GROUP_NAME environment variable

## CodeStar
* integrated solution to regroup all the CICD services
* wrapper around everything to EC2, Lambda, Beanstalk
* Issue tracking with Jira and Github Issues
* Ability to integrate with Cloud9 as IDE
* One dashboard for all components
* free service, pay for underlying services
* limited customization# CloudFormation

## Intro
* Infrastructure as code: the code that we write will create and update infrastructure
* this is a declarative way of outlining our AWS Infrastructure
* We can manage almost any resources with it
* Benefits:
	1. no manual creation
	2. Code can be version controlled
	3. changes in infrastructure can be reviewed as cod
* cost can be estimated using CloudFormation template
* productivity:
	* create and delete infrastructure automatically
	* can save a lot of money
	* uses declarative programming; so no need to figure out ordering
* separation of concern: many stacks with many apps and many layers
* there are already many templates online
* upload the template to S3 and reference in CloudFormation
* to update, need to upload new template
* stacks are identified by name
* deleting stacks deletes everything underneath
* Ways of deployment:
	1. Manual:
		* edit in cloudformation
		* use console to input parameters
	2. automated (recommended):
		* edit templates in YAML file
		* use AWS CLI to deploy templates

## building block
* templates components
	* resources: 
		* mandatory section. 
		* aws resources
		* can refer each other
		* over 224 types of resources; not all are supported yet
		* `AWS::aws-product-name::data-type-name`
		* Reference: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html
		* sample: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html
	* parameters: dynamic inputs for templates
		* way to provide inputs
		* useful if template is reused
		* some inputs can't be known ahead of time
		* if a resource config is likely to change, make it a parameter
		* parameters:
			* Types:
				* String
				* Number
				* CommaDelimitedList
				* List<Type>
				* AWS Parameter
			* Descriptions
			* Contraints
			* ConstraintDescription (string)
			* Min/MaxLength
			* Defaults
			* AllowedValues(array)
			* AllowedPattern(regex)
			* NoEcho(Boolean)
		* Pseudo Parameters (AWS provided)
			* AWS::AccountId
			* AWS::NotificationARNs
			* AWS::NoValue (doesn't return a value)
			* AWS::Region
			* AWS::StakId
			* AWS::StackName
	* mappings: static variables for template
		* hard coded variables
		* handy to differentiate between environments (prod/dev), regions, AMI types
		* Example:
		```
		Mappings:
			Mapping01:
				Key01:
					Name: Value01
				Key02:
					Name: Value02
		```
		* great for values known in advance
		* safer control over the template
		* access the map:`Fn:FindInMap` or `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]`
		* Example: `!FindInMap [ Mapping01, Key01, Name ]`
	* outputs: reference to what has been created
		* optional
		* if we output something that can be used in other templates
		* can be viewed in AWS Console or CLI
		* For example: VPC/Subnet IDs
		* A stack that has a reference somewhere else can't be deleted
		* exported output name must be unique within the region
		* Export:
		```
		Outputs:
			StackSSHSecurityGroup:
				Description: Something
				Value: !Ref SomethingElse
				Export:
					Name: SSHGroup
		```
		* Import: using `Fn:ImportValue` or `!ImportValue`
		```
		!ImportValue SSHGroup
		```
	* conditionals: list of conditions to perform resource creation
		* control creation or resource or output based on condition
		* commonly conditions are:
			* environment (dev/prod)
			* AWS Region
			* Any Parameter
		* one condition can reference other conditions, parameter, mapping
		* intrinsic functions can be:
			* Fn::And
			* Fn::Equals
			* Fn::If
			* Fn::Not
			* Fn::Or
	* metadata 
	* Reference of Intrinsic Functions: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html
* templates helpers
	* references
	* functions

## CloudFormation Rollbacks
* Stack Creation Fails:
	* Default: everything rolls back (gets deleted). Check log
	* Option to disable rollback
* Update Fails:
	* Default: rolls back to previous known working state

## Other Concepts
* ChangeSets
	* don't tell whether a update will be successful or not
	* Originl Stack > create change set and view > execute change set to create new update
* Nested Stacks
	* part of other stacks
	* isolate repeated parts
	* LB config that is reused
	* Considered best practice
	* must update the parent to update the nested stack
* Cross Stack vs Nested Stack
	* Cross Stack
		* helpful if stacks have different lifecycles
		* Use Outputs Export and Fn:ImportValue
	* Nested Stack
		* one component is reused
		* importnat only to higher level stack (not shared)
* StackSets
	* Create/Update/Delete stacks across multiple accounts and regions with single operation
	* Need Administrator to create it
	* Trusted accounts can Create/Update/Delete stack instances inside StackSet
	* If a stack is updated, all associated stack instances are updated# Monitoring, Troubleshooting, and Audit

## Monitoring 
* CloudWatch
	* Metrics: collect/track metrics
	* Logs: collect/monitor/analyze/store log files
	* Events: send notifications
	* Alarms: react in real-time to metrics/events
* X-Ray:
	* troubleshooting
	* distributed tracing of microservices
* CloudTrail:
	* Internal monitoring of API calls
	* audit changes to AWS resources by users

## CloudWatch
* Metric is a variable to monitor
* metrics are grouped by namespaces
* Dimension is an attribute of a metric (eg instance id)
* upto 10 dimensions per metric
* Metrics have timestamps
* can create dashboards
* EC2 Detialed Monitoring:
	* default every 5 minutes
	* can be set to 1 minute with additional cost
	* EC2 Memory usage is not pushed by default but can be setup as custom metric
* Custom Metrics
	* can set up custom metrics
	* can use dimensions to segment metrics
	* Metric resolution:
		* standard: 1 minute
		* High Resolution: upto 1 second with higher cost(API Call is called StorageResolution)
	* To send a metric call PutMetricData API
	* if sending too much data use exponential back off
* Alarms
	* trigger notification by metrics
	* can be attached to EC2 Actions or SNS Notifications
	* Stages:
		1. OK
		2. INSUFFICIENT_DATA
		3. ALARM
	* Period:
		* length of time in seconds to evaluate metrics
		* with High Resolution can choose 10 or 30 sec
* Logs
	* applications can send logs to cloudwatch using SDK
	* CloudWatch can collect log from:
		* Elastic Beanstalk (application logs)
		* ECS (from containers)
		* AWS Lambda (function logs)
		* VPC Flow Logs
		* API Gateway
		* CloudTrail based filter
		* CloudWatch log agents: eg EC2 machines
		* Route53: Log DNS queries
	* Processed in cloudwatch/stored in S3/streamed to ElasticSearch cluster
	* can use filter expressions
	* storage architecture:
		* Log Groups: arbitrary names for application
		* Log stream: instances within app/log files/containers
	* can define log expiration policy (how many days)
	* can use AWS CLI to tail logs
	* need to make sure IAM permissions are correct
	* encrypted with KMS for logs at rest
* CloudWatch Agent
	* by default not logs are going from EC2 instances
	* need to run Cloudwatch Agent to push log files
	* need to make sure IAM permissions are correct
	* the Agent can be setup on-premises too
	* there are two agents:
		* logs agent (old): can only send to CloudWatch Logs; by default high level
		* unified agent: collect additional system level metrics; centralized config using SSM Parameter Store
			* CPU: active, gues, idle, system, user, steal
			* Disk Metrics
			* Disk IO
			* RAM
			* Netstat: number of TCP/UDP connections, net packets, bytes
			* Processes
			* Swap space
* CloudWatch Logs Metric Filter
	* can use filter expressions
		* find specific IP
		* Count something
		* can be used to trigger alarm
	* not retroactive; only publish data points after the filer was created
	* Filter Reference: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html
* CloudWatch Events
	* can schedule with cron jobs
	* can define event pattern (eg CodePipeline state changes)
	* can trigger to Lambda functions, SQS/SNS/Kinesis messages
	* creates a small JSON document

## Amazon EventBridge
* next evolution of CloudWatch Events
* Default event bus: generated by AWS Services
* Partner Event bus: receives events from SaaS (Zendesk, DataDog, Segment, Auth0)
* Custom event bus: own applications
* possible to do cross account event 
* Can create rules
* EventBridge can analyze events in bus and infer schema (data structure), which can allow code for app 
* Schema can have versions
* builds on CloudWatch Events

## AWS X-Ray
* Visual analysis of the application
* can troubleshoot performance (bottleneck)
* understand dependencies in a microservice architecture
* pinpoint issues
* review request behaviors
* find errors and exceptions
* Compatibility:
	* Lambda
	* beanstalk
	* ECS
	* ELB
	* API Gateway
	* EC2 and on-premise
* Tracing:
	* end-to-end way to following a request
	* each component dealing with the request add it's own trace
	* tracing is made of segments and sub-segments
	* annotations can be added to traces for extra-information
* security:
	* IAM for auth
	* KMS for encryption at rest
* how to enable:
	1. Code must import AWS XRAY SDK
	2. install XRAY daemon or enable XRAY integration in AWS
* X-Ray troubleshooting
	* not working on EC2
		* check IAM role
		* Check X-Ray Daemon
	* AWS Lambda
		* check IAM execution role with proper policy (AWSX-RayWriteOnlyAccess)
		* check X-Ray is imported in code
* APIs
	* Write API
		* PutTraceSegments: Uploads segment documents
		* PutTelemetryRecords: upload telemetry (SegmentsReceivedCount, SegementsRejectedCounts, BackendConnectionErrors)
		* GetSamplingRules: Retrieve all sampling rules to know sampling rules are changing
		* GetSamplingTargets
		* GetSamplingStatisticSummaries
	* Read API
		* GetSamplingRules
		* GetSamplingTargets
		* GetSamplingStatisticSummaries
		* BatchGetTraces: list of traces; each trace is a segment of documents
		* GetServiceGraph: main graph
		* GetTraceGraph: get a specific service graph 
		* GetTraceSummaries: get IDs and annotations for traces available
		* GetGroups
		* GetGroup
		* GetTimeSeriesServiceStatistics
* Integrate with Beanstalk
	* beanstalk includes X-Ray Daemon
	* the daemon can be run setting an option in EB Console or config file (`.ebextensions/xray-daemon.config`)
	* make sure instance profile has correct IAM
	* make sure the code includes X-Ray SDK 
	* not provided for multi-docker containers
* Integrate with ECS
	1. Use X-Ray container as daemon. daemon will run in all EC2 instances
	2. Side car Pattern: extra container with each app container
	3. For Fargate Cluster must be used as Side Car Pattern 

## X-Ray Instrumentation
* means the measure of product's performance, diagnose errors, and to write trace information
* to instrument, need to use X-Ray SDK
* can use either interceptors, filters, handlers, middlwares based on the code
* Concepts:
	* segments: each app/service will send
	* subsegments: more details
	* trace: segments collected together to form end-to-end trace
	* sampling: decrease amount of requests sent to x-ray; reduces cost
		* control the amount of data
		* Default: records first request each second (reservoir) and 5% of additional requests (rate)
	* Annotations: key value pairs to index traces and filter them
	* Metadata: key value pairs for searching; not indexed

## CloudTrail
* provides governance, compliance and audit
* enabled by default
* get history of account by:
	* console
	* SDK
	* CLI
	* AWS Services
* Can put logs from CloudTrail to CloudWatch Logs
* if someone did something, look into CloudTrail First

## CloudTrail vs CloudWatch vs X-Ray
* CloudTrail is used to audit API calls by users/services/Consoles; helpful to detect changes
* CloudWatch for monitoring and logs
* X-Ray auto trace analysis and central service map visualization; latency, error, fault analysis# Integration and Messaging

## Intro
* interconnected applications will need to communicate among them
* Two patterns of application communication:
	1. Synchronous communication (application to application)
	2. Asynchronous/Event based Communication (app to queue to app)
* ways to decouple apps:
	1. SQS: queue model
	2. SNS: pub/sub model
	3. Kinesis: real-time streaming model

## Amazon SQS
* This is a queue service
* producers send messages to the SQS Queue
* Consumers poll messages from the queue and processes
* There can be multiple producers and consumers
* Fully managed service used to decouple applications
* attributes:
	* unlimited throughput
	* unlimited number of message in queue
	* default retention is 4 days; max 14 days
	* Low latency (<10 ms)
	* limitation of 256kb/message
* "at least once delivery" can deliver messages multiple times
* can have out of order messages (best effort ordering)
* Producing messages:
	* sent using SDK (SendMessage API)
	* message would be persisted until a consumer deletes it
* Consumers:
	* running on EC2, AWS Lambda, On-premise
	* poll SQS messages
	* receive upto 10 messages at a time
	* process the message
	* deletes the messages using DeleteMessage API
* Multiple Consumers:
	* consumers receive and process messages parallely
	* at least once delivery
	* best effort message ordering
	* delete messages after deleting
	* increase consumers to increase throughput (horizontal scaling)
* ASG can be used for consumers with QueueLenght (ApproximateNumberOfMessages) as metric
* Security
	* in-flight encryption using HTTPS API
	* At-rest encryption using KMS
	* client-side encryption need to be performed by the client
	* Access Control through IAM
	* SQS Access Policy for cross-account access or for other services
* FIFO Queues
	* when order of the messages matter
	* first message received will be consumed first
	* the queue name has to end with `.fifo`
	* limited throughput
	* 300 messages/second without batching
	* 3000 messages/second with batching
	* exactly one send capability (remove duplicates)
	* Deduplication
		* deduplication interval is 5 mintues (duplicates can't be sent within this time)
		* two types of deduplication:
			* content-based (using SHA-256 hash)
			* ID based (message deduplication id is provided)
	* Message Grouping
		* one MessageGroupID mean it will be consumed by one consumer and all messages will in order
		* to get ordering at the level of subset of messages, specify different values
		* ordering across group is not guaranteed
		* messages with one group id will have order

## SQS Options
* Message Visibility Timeout
	* a polled message is invisible to other consumers
	* this is the time length by which a message has to be processed
	* after the message visibility timeout, message will be visible again
	* if it is not processed within this period, the consumer can call ChangeMessageVisibility API to get more time
	* don't set too long or too short
* Dead Letter Queue
	* consumer couldn't process a message within Visibility timeout and so the message goes back the queue. If this happens too many times (MaximumReceives), the message goes into Dead Letter Queue (DLQ)
	* useful for debugging
	* can be analyzed later
	* this will expire some time. good to set it to 14 days
 * Delay Queue
 	* delay a message up to 15 minutes
 	* default is 0 seconds
 	* can set default at queue level
 	* can be overriden using DelaySeconds parameter
 * Long Polling
 	* if queue is empty, consumer can wait for a message to arrive
 	* reduce API calls
 	* increases efficiency and decreasing latency
 	* set between 1 and 20 second
 	* preferrable over Short Polling
 	* can be enabled at Queue level or API level (WaitTimeSeconds)
 * Extended Client
 	* max message size is 256kb
 	* to overcome this, we can use SQS Extended Client (Java Library)
 	* S3 is used a repository for the message
 	* message will contain metadata of the actual message in S3
 * APIs
 	* CreateQueue
 	* DeleteQueue
 	* PurgeQueue
 	* SendMessage(DelaySeconds)
 	* ReceiveMessage
 	* DeleteMessage
 	* ReceiveMessageWaitTimeSeconds
 	* ChangeMessageVisibility
 	* There are batch APIs for send, delete, change message visibility

## Amazon SNS
* this follows the pub/sub pattern or publisher/subscriber pattern
* multiple apps can subscrib to SNS Topic
* when a publisher publishes a message to the Topic all the subscriber will get message from that Topic
* the event producer sends message to one SNS Topic
* the event receivers (subscribers) will get the message
* there is a way to filter the messages as well
* can have up to 10 million subscriptions per topic
* there can be up to 100,000 topics
* Subscribers can be:
	* SQS
	* HTTP/HTTPS (with delivery retries)
	* Lambda
	* Email notification
	* SMS notification
	* Mobile notification
* Can integrate with a lot of services, including:
	* CloudWatch (alarms)
	* ASG
	* S3 buckets
	* CloudFormation (state change > failed to build)
* How to publish:
	* Topic Publish (with SDK)
		* create a topic
		* create subscriptions
		* publish to topic
	* Direct Publish (mobile apps SDK)
		* create platform app
		* create platform endpoint
		* publish to platform endpoint
		* works with Google GCM, Apple APNS, Amazon ADM
* Security:
	* in-flight with HTTPS API
	* At-rest with KMS
	* client encryption upto client itself
	* Access Control with IAM policies
	* SNS Access Policies for Cross-access or other services
* Subscription protocols:
	* HTTP
	* HTTPS
	* Email
	* Email-JSON
	* Amazon SQS
	* AWS Lambda

## SNS + SQS Fan Out
* push messages to multiple SQS Queues
* Push once, receive in multiple SQS Queues
* Fully decoupled, no data loss
* SQS allows for data persistence, delayed processing and retries of work
* SQS queue need to have access policy
* **SNS can't send messages to SQS FIFO Queues**
* For example: S3 Event rule can send notification to only one rule; so we can connect a SNS topic to S3 Event

## AWS Kinesis
* managed alternative to Apache Kafka
* big data streaming tool for app logs, metrics, IoT, clickstreams
* for real-time big data
* great for stream processing frameworks (Spark, NiFi)
* data auto replicated to 3 AZs
* Three kinesis Products:
	1. Kinesis Stream: low latency streaming ingest
	2. Kinesis Analytics: real-time analysis with SQL
	3. Kinesis Firehose: load streams into other parts of AWS: S3, Redshift, ElasticSearch
* produced data goes into Streams, which goes through Analytics, which is stored into S3 or other services through Firehose

## AWS Kinesis Streams
* divided into ordered Shards/Partitions, similar to queue
* data coming in goes into shards and directed to consumers
* to scale up need to incease number of shards
* data retention in shards is 1 day by default, max 7 days
* able to reprocess/replay data
* multiple app can consume the same stream
* real-time processing with scale of throughput
* data in kinesis is immutable and can't be deleted
* Shards:
	* stream is made of shards
	* 1MBps or 1000 messages/s at write per shard
	* can read 2MBps per shard
	* if more MBps is needed, need to increase number of shards
	* billing is as per number of shards provisioned
	* batching available or per message calls
	* number of shards can evolve (reshard: adding/merge: reducing)
	* records are ordered per shard
	* PutRecord API to send data to shards through Partition Key (which is hashed); same key goes to same shard
	* messages sent get a sequence number, which is always increasing
	* needs to choose a highly distributed Partition Key to avoid "hot parition" (to make sure data goes to different shards rather than just one/few)
	* user_id can be a good partition key; country_id may not be a good parition key if most of the users are from a country
	* can use batching
	* for going over the limit, would receive ProvisionedThroughputExceeded (use retries and exponential back off to avoid this or reshard)
* Consumers:
	* can use a normal consumer (CLI/SDK)
	* can use Kinesis Client Library (Java, Python, Node, Ruby, .Net)
	* KCL uses DynamoDB to checkpoint offset and track other workers and share works among shards
	* KCL basically enables consumers to consume from kinesis efficiently	
* Kinesis KCL:
	* Kinesis Client Library
	* a java library
	* helps to read records from a Stream with distributed applicatioins sharing the read workload
	* **each shard must be read by only one KCL instance; number of KCL can be lower**
	* the progress of how far it has been reading into Kinesis is checkpointed at DynamoDB (need IAM acess)
	* KCL can run on: EC2, EB, on-premise
	* records will be read in order at shard level
	* if the number of KCL is lower than Shards, to scale up we can increase the number of KCL first. If after number of KCLs reaches maximum, we need more throughput, we need to increase number of Shards
* Security:
	* access: IAM Policy
	* in-fligh: HTTPS
	* at-rest: KMS
	* client encrypt/decrypt
	* VPC Endpoints for Kinesis within VPC
* Kinesis Data Analytics
	* real-time analytics using SQL
	* its auto scaling
	* managed; no servers to provision
	* continuous; real-time
	* pay for consumption
	* can create streams out of real-time queris
* Firehose:
	* fully managed
	* near real time (60 second latency)
	* load data into Redshift/S3/ElasticSearch/Splunk
	* automatic scaling
	* support many data format (need to pay for conversion)
	* pay for amount of data going through Firehose

## SNS vs SQS vs Kinesis
* SQS:
	* consumer pull data
	* data is deleted after consumption
	* can have many workers
	* no need to provision throughput
	* no ordering (except FIFO)
	* individual message delay capability
* SNS:
	* pub/sub model
	* push data to many subscribers
	* data not persisted
	* upto 100,000 topics
	* no provisioning for throughput
	* integrates with SQS for Fan Out
* Kinesis:
	* consumer pull data
	* as many consumers as we need
	* possibility to replay data
	* for real-time, big data, analytics, ETL
	* ordering at shard level
	* data expires after few days
	* must provision throughput


# AWS Lambda

## Intro
* Serverless is when you don't have to manager server; just deploy and it works. Initially, serverless ment Function as a Service (FaaS)
* Serverless Technologies include:
	* AWS Lambda
	* DynamoDB
	* AWS Congnito
	* AWS API Gateway
	* Amazon S3
	* AWS SNS
	* AWS SQS
	* AWS Kinesis Data Firehose
	* Aurora Serverless
	* Step Functions
	* Fargate

## AWS Lambda
* virtual functions; no servers to manage
* limited by time - short executions
* run on demand
* billed for running 
* scaling automated
* Benefits:
	* easy pricing
	* pay per request and compute time
	* free tier of 1 million AWS Lambda Requests and 400,000 GBs of compute time
	* integrated with AWS suites of services
	* integrated with many programming languages
	* easy monitoring through AWS CloudWatch
	* can get upto 3GB of RAM per function
	* updating RAM will update CPU and Network
* Supported Languages:
	* Node
	* python
	* Java
	* C#/(.Net Core)
	* Golang
	* C#/Powershell
	* Ruby
	* Custom Runtime API (community supported, ex: Rust)
* **Docker doesn't run on lambda**
* Pricing:
	* pay per calls
		* first 1 million requests are free
		* after that per 1 million requests cost $0.20
	* Pay per Duration (incremnet of 100ms):
		* 400,000 GBs of compute time per month is free
		* 400,000 seconds of execution for 1GB RAM
		* after that $1 for 600,000 GBs

## Lambda - Synchronous Invocation
* done through: CLI, SDK, API Gateway, ALB
* results are returned right away
* client will be waiting for the response
* error handling must happen client side (retires, exponential backoff)
* services that are synchronous:
	* use invoked:
		* ELB
		* API Gateway
		* CloudFront(Lambda@Edge)
		* S3 Batch
	* Service invoked:
		* Cognito
		* Step Functions
		* Lex
		* Alexa
		* Kinesis Data Firehose

## Lambda Integration with ALB
* to expose lambda function to HTTP(S) endpoint: 
	* can use Application Load Balancer (or API Gateway); 
	* Lambda must be registered in Target Group
	* client will send requests to ALB in HTTP
	* ALB will forward the request to Lambda and receive response from it
	* Finally, forward the response to client
	* HTTP request gets converted into JSON
	* query string paramters and headers are converted to key-value pairs
	* Also, ALB convers the response from JSON to HTTP
	* Multi-Header Values can be enabled at ALB settings;
	* Multi-header values in query string paramter are converted to arrays within the Lambda event and response objects

## Lambda@Edge
* another synchronous deployment of Lambda
* deploy Lambda functions not in a specific region but alongside each region with CloudFront
* Lambda will be deployed globally
* Customize CDN content
* Pay for use
* Lambda can be used to change CloudFront request and responses
	* modify reqeusts from a viewer (viewer request)
	* modify requests to origin (origin request)
	* modify response from origin (origin response)
	* modify response to the viewer (viewer response)
	* can generate responses without even sending requests to origin
* we can easily make global applications like:
	1. static website in S3
	2. User visits website and JS in 
	3. website does API request to CloudFront
	4. CloudFront invokes Lambda@Edge function
	5. Lambda@Edge queries data from DynamoDB
* Benefits:
	* security and privacy
	* Dynamic web application at edge
	* SEO
	* intelligently route across origins and Data centers
	* bot mitigation at edge
	* real-time image transformation
	* A/B Testing
	* User authentication and authorization
	* user prioritization
	* user tracking and analytics

## Lambda - Asynchronous Invocations
* asynchronous invocation can be done using:
	* S3
	* SNS
	* CloudWatch Events or EventBridge
	* CodeCommit
	* CodePipeline
	* Simple Email Service
	* CloudFormation
	* AWS Config
	* AWS IoT
	* AWS IoT Events
* S3 Bucket may have New File Event which will place place in Event Queue
* Lambda Function will be reading and processing from that event queue
* if something goes wrong, Lambda will retry (3 times)
	* 1st one right away
	* 2nd one 1 min after 1st one
	* 3rd one 2 min after 2nd one
* because of the retries processing need to be idempotent (output the same result every time)
* during reties, may see duplicate log entries in CloudWatch
* can define a DLQ (Dead Letter Queue) - SNS or SQS - for failed processing (need IAM permission)

## Lambda with CloudWatch Events or EventBridge
* there are two ways:
	1. serverless cron or Rate: trigger every one hour to invoke Lambda
	2. CodePipeline Rule: on state changes invoke Lambda

## Lambda with S3 Event Notification
* get notified when something happens in S3
* notification can have three patterns:
	1. S3 to SNS to Fan out to SQS
	2. S3 to SQS to Lambda
	3. S3 to Lambda to DLQ (SQS)
* for notification of every successful write, enable versioning

## Lambda - Event Source Mapping
* applies to:
	* Kinesis Data Streams
	* SQS and SQS FIFO Queue
	* DynamoDB Streams
* Lambda need to messages/data from these services
* Lambda invoked synchronously
* Event Source Mapping is going to poll from sources and will get batches. Then it will invoke Lambda with Event batch.
* Categories:
	1. Streams
		* Kinesis Streams and DynamoDB Streams
		* event source mapping creates iterator for each shard
		* Processes items in order at shard level
		* Start from beginning or a specific timestamp
		* items are not removed from stream
		* low traffic: use batch window for accumulate records before processing
		* can process multiple batches in parallel
		* one lambda invocation per stream shard
		* upto 10 batches per shard
		* by default, if a batch process fails, it is reprocessed until success or items expire
		* to ensure in-order processing, the affeced shard is paused until error is resolved
		* event source mapping can be configured to:
			* discard old events
			* restrict number of retries
			* split the batch on error
		* Discarded events can go to a Destination
	2. Queues
		* SQS and SQS FIFO
		* polled by Event source mapping using Long Polling
		* can specify batch size (1-10 messages)
		* Standard queue: lambda adds 60 more instances per minute to scale up; upto 1000 batches of messages processed simultaneously
		* FIFI queue: messae with same groupID will be processed in order; scales up to the number of active message groups
		* Recommended to set the queue visibility timeout to 6 times of Lambda Function timeout
		* To use DLQ, setup on SQS Queue not on Lambda (DLQ for Lambda only for async invocation)
		* Lambda Destition can also be used
		* for SQS FIFO, Lambda scales up to number of active message groups
		* not in-order for standard queue
		* Lambda scales up to process standard queue ASAP
		* on error, batches are returned to queue as individual items and may be processed in different groups
		* event source mapping may receive an item twice even without error (prevented by idempotency)
		* deleted from the queue after processing
		* source queue can be configured to send items to DLQ

## Lambda Destinations
* can configure to send results to a destination
* asynchronous invocation: destination can be used for successful or failed events
	* services can used:
		* SQS
		* SNS
		* Lambda
		* EventBridge Bus
	* recommendation is to use Destination instead of DLQ (both can be used at the same time)
* Event Source Mapping: for discarded event batches
	* services:
		* SQS
		* SNS
	* with SQS, both DLQ or Destination can be setup

## Lambda Execution Role (IAM Role)
* role must be attached to services
* there are many managed policies for Lambda
* for event source mapping, lambda uses execution role to read event data
* Best practice: create one Lambda Execution role per function
* If other resources use Lambda, then we need to use Resource based policies to let other services use Lambda resources
* IAM Principal can access Lambda:
	* if IAM policy attached to principal authorizes it
	* or if we have resource based policy

## Lambda Environmet Variables
* these are key-value pairs in string format
* help adjust function without updating code
* these are available to code as well
* lambda adds its own variables as well
* these can be used to store secrets
* secrets can be encrypted by Lambda Service Key or own CMK

## Lambda Loggin & Monitoring
* integrated with CloudWatch Logs to store Lambda execution logs with proper IAM policy
* Lambda metricsa re displayed in CloudWatch Metrics: 
	* invocations, 
	* durations, 
	* concurrent executions
	* Error count
	* Success rates
	* Throttles
	* Async delivery failures
	* Iterator age (Kinesis and DynamoDB Streams)
* Tracking can be done with X-Ray, called Active X-Ray
* runs X-Ray Daemon automatically
* use X-Ray SDK in code
* ensure Lambda has correct IAM (AWSXRayDaemonWriteAccess)
* There are environment variables to communicate with X-Ray:
	* `_X_AMAZN_TRACE_ID`: contains the tracing header
	* `AWS_XRAY_CONTEXT_MISSING`: by default, `LOG_ERROR`
	* `AWS_XRAY_DAEMON_ADDRESS`: X-Ray daemon `IP:PORT` (most important)

## Lambda in VPC
* by default, launched outside default VPC. so, can' access resources within the default VPC
* to deploy within VPC, we need to define VPC ID, subnets, and secuirty groups. Lambda will create an ENI in the subnets. 
* to create this ENI, Lambda will require `AWSLambdaVPCAccessExecutionRole`
* by default, Lambda inside default VPC doesn't have internet
* deploying Lambda in public subnet will NOT give it internet or public IP
* to give it internet access, we need to deploy in private subnet and give it NAT Gateway/Instance (configed through route tables)

## Lambda Performance
* Configuration
	* RAM: default 128MB; max 10240MB; increases vCPU with RAM; 1,792MB equal to 1 full vCPU
	* to handle computation heavy application (higher CPU), increase RAM
	* default timeout of 3 seconds; max 900 seconds (15 mintutes);
* execution context
	* temporary runtime environment that initializes external dependencies of our code
	* great for database connections, HTTP clients, SDK clients
	* maintained for some time in anticipation of another invocation (reused)
	* includes `/tmp` directory
* if something needs to be done again and again, its better to put it outside the function handler
* temporary files:
	* use `/tmp` space
	* good to download big file to work or if lambda needs disk space to perform something
	* max size is 512MB
	* the content remains when the context is frozen, providing transient cache to be used by multiple invocations;
	* this could be helpfult to checkpoint work
	* for permanent persistence, use S3

## Lambda Concurrency and Throttling
* concurrency limit of 1000 executions
* "Reserved Concurrency" at functional level can be set to restrict number of concurrent executions
* each invocation over this limit will trigger a "Throttle"
* Throttle behaviors:
	* synchronous invocation: return ThrottleError - 429
	* Asynchronous invocation: retry automatically and go to DLQ
* for over 1000 limit, open a support ticket
* issues:
	* if not reserve concurrency is set, one function may get more executions and other functions may be chocked
	* if not enough concurrency available for async invocations, additional reqeusts will be throttled. 
	* async events: Lambda will return the event to queue (throttling error: 429, and system error: 500s) and try to run it again for upto 6 hours; retry time increases from 1sec to max 5 minutes (exponential backoff)
* Cold starts and Provisioned Concurrency:
	* when new Lambda instance is created, the code needs to be loaded and code outside the handler need to be run first (init)
	* if the 'init' is large, this can take some time
	* so, the first request served have higher latency than the rest (cold start)
	* This can be overcome by Provisioned Concurrency
	* concurrency is allocated before the function is invoked
	* Application auto scaling can manage concurrency (schedule or target utilization)
	* additional resource: https://aws.amazon.com/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/

## Function Dependencies
* if the function depends on external libraries (AWS X-Ray SDK, Database Clients), we will need to install the packages along code and **zip it together**
* example:
	* node.js: npm and 'node_modules' directory
	* python: use `pip --target` options
	* java: include .jar files
* upload the zip file to Lambda, if less than 50MB; otherwise S3 first and reference in code
* native libraries work; need to be compiled on Amazon Linux
* AWS SDK comes default with all Lambda Functions

## Lambda and CloudFormation
* Inline
	* very simple use case
	* within our CloudFormation template
	* need to use the Code.ZipFile property of the template
	* can't include function dependencies
* Through S3
	* store the Lambda Zip in S3
	* refer the zip location in CloudFormation code:
		* S3Bucket
		* S3Key (full path to zip)
		* S3ObjectVersion (if versioned bucket)
	* need to manually update version in Lambda

## Lambda Layers
* use cases:
	* enable to create Custom Runtimes; C++ and Rust
	* Externalize dependencies to re-use them
* The way is to create a layer for Lambda. The layers can be referenced from the function. This way we don't need to repackage everthing each time we update the version of app
* Additional reading: https://aws.amazon.com/blogs/aws/new-for-aws-lambda-use-any-programming-language-and-share-common-components/

## Lambda Versions and Alias
* Versions: 
	* initially working with Lambda Function, the version is $LATEST (this is mutable)
	* when we're ready to publish a Lambda function, we can create a version
	* versions are immutable (cant change code or environment variables)
	* versions will have increasing number
	* versions get their own ARN (Amazon Resource Number)
	* Version = Code + configuration
	* each version of lambda can be accessed
* Aliases:
	* pointers to versions
	* we can define dev/test/prod aliases and have them point at different lambda versions
	* they are mutable
	* can be used with Blue/Green deployment
	* enable stable configuration of event triggers/destinations
	* Aliases have their own ARNs
	* Aliases can't point to other aliases

## Lambda and CodeDeploy
* can help automate traffic shift for lambda aliases
* integrated within SAM (Serverless Application Model) framework
* CodeDeploy can grow traffic on an alias gradually over time shifting from another one
	* Linear:
		* Linear10PercentEvery3Minutes
		* Linear10PercentEvery10Minutes
	* Canary (try N percent and switch to 100%):
		* Canary10Percent5Minutes
		* Canary10Percent30Minutes
	* AllAtOnce: immediate
* can create Pre and Post traffic hooks to check health and Rollback on that

## Lambda Limits
* per region
* Execution:
	* memory 128 - 3008MB (64MB increment)
	* Max execution time: 900 seconds (15 minutes)
	* Environment variables (4kb)
	* function container, /tmp direcotry (512MB)
	* Concurrency executions: 1000 (can be increase by request)
* Deployment:
	* Deployment size: 50MB (Compressed), 250 (uncompressed)
	* can use /tmp directory
	* environment variable: 4kb

## Best Practices
* heavy duty and repeatative tasks outside handler
* environment variable for sensitive information (can be encrypted as well)
* Minimize deployment package size to runtime requirements (break down big functions)
* use lambda layers to manage dependencies
* avoid using recursive code!!# DynamoDB

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
* DynamoDB can be launched locally on computer# API Gateway

## Intro
* can provide REST API Gateway to Lambda function fo the client
* Lambda + Gateway can build a complete serverless application
* has support for WebSocket Protocol
* handle API versioning
* handle multiple environments
* handle security
* create API key, handle request throttling
* Swagger/Open API import to quickly define APIs
* Transform and validate requests and responses
* Generate SDK and API specifications
* Cache API responses
* Integrate with:
	* Lambda Function
	* HTTP (any HTTP endpoints)
	* AWS Services API
* Types:
	* Edge-optimized (default)
		* for global clients
		* requests are routed through CloudFront Edge locations
		* the Gateway still lives in one region
	* Regional
		* client in same region
		* can manually combine with CloudFront (more control over caching strategies and distribution)
	* Private
		* can only be accessed from within VPC using ENI
		* use resource policy to define access
* Deployment
	* need to deploy API to make them effective
	* changes are deployed to stages
	* each stage has their own configuration parameters
	* stages can be rolled back because history of deployments is kept
* Stage Variables
	* these are like environment variables
	* passed to the "context" object of Lambda function
	* use them to change often changing configuration values
	* can be used in:
		* Lambda Function ARN
		* HTTP Endpoint
		* Parameter mapping templates
	* Use cases:
		* configure HTTP endpoints the stage talk to (dev/prod)
		* pass config parameters to AWS Lambda through mapping templates
		* use this to indicate the corresponding Lambda alias which will point to a Lambda Function

## Canary Deployment
* possibility to enable canary deployment for any stage
* choose % of traffic the canary channel receives
* metrics and log will be separate
* possibility of override stage variables for canary
* blue/green deployment with Lambda and Gateway

## Integration
* Integration Type MOCK
	* mock integration
	* Gateway returns a response without sending the request to backend
* Integration Type HTTP/AWS
	* must configure both integration request and response
	* setup data mapping using mapping templates for request and response
* AWS_PROXY (Lambda Proxy)
	* reqeusts from client is input to lambda
	* function is responsible for logic of request/response
	* no mapping template, headers, query string parameters; these are passes as arguments
* HTTP_PROXY
	* no mapping template
	* HTTP request is passed to the backend
	* the response is forwarded by API Gateway
* Mapping Templates
	* for AWS and HTTP integration
	* can be used to modify request/response
	* rename/modify query string parameters
	* modify body content
	* add headers
	* uses Velocity Template Language (VTL), which is scripting language
	* can filter out results
	* Use case: convert SOAP to REST; map query string parameter to json document

## API Swagger/Open API spec
* common way fo defining REST APIs, using API definition as code
* Import existing Swagger/Open API spec to API gateway, includes:
	* method
	* method request
	* integration request
	* method response
	* AWS extensions of API Gateway
* can export current API as Swagger/Open API spec
* can be written in YAML or JSON
* can generate code for app using Swagger

## Caching API Responses
* API gateway will get responses from the cache
* Default TTL is 300 seconds; max 3600s
* Caches are defined at stage level
* can be override at method level
* can encrypted
* size between 0.5GB to 237GB
* cache is expensive
* makes sense in Prod
* can flush the entire cache (invalidate) immediately
* clients can invalidate cache with `header: Cache-Control:max-age=0` (need IAM auth)
* not imposing InvalidateCache policy can enable any client to invalidate the cache

## Usage Plans and API Keys
* can make API available as an offering to customers
* Usage plan:
	* who can access one or more deployed API stages and methods
	* how much and how fast they can access
	* use API keys to identify clients and meter access
	* configure throttling limits and quota limits that are enforced on individual client
* API Keys:
	* strings distributed to customers to authenticate users
	* can be used with usage plan
	* throttling limits are applied to API keys
	* Quotas limits is overall number of maximum requests
* configure usage plan
	1. create one or more APIs, config methods to require keys, and deploy APIs
	2. generate or import api keys to distribute to develpers (customers)
	3. create usage plant with desired throttle and quota limits
	4. associate API stages and keys with usage plan
* must provide the key in `x-api-key` header of request

## Monitoring and Logging
* CloudWatch Logs
	* enable logging at stage level
	* can override at method level
* X-Ray
	* enable to get extra information
	* X-Ray on Gateway + Lambda give full picture
* CloudWatch metics
	* metrics are by stage
	* possible to enable detailed metrics
	* major metrics: 
		* `CacheHitCount` 
		* `CacheMissCount`
		* `IntegrationLatency`: time between relay request and receives response from backend
		* `Latency`: time between receiving a request and sendin resopnse from client; more than `IntegrationLatency`
	* errors: 
		* 4xxErrors (client side)
			* 400: bad request
			* 403: access denied
			* 429: too many reqeust
		* 5xxErrors (server side)
			* 502: bad gateway exception
			* 503: service unavailable
			* 504: integration failure (endpoint reqeust timed out)
* throttling
	* throttles requests at 10000 reqeusts per second accross all API
	* can be requested to increase
	* error response: 429 (too many reqeusts)
	* improve performance by Stage limit and method limits or usage plans
	* one overloaded API can hamper others

## CORS
* can be enabled API calls from another domain
* options pre-flight request must contain following headers:
	* `Access-Control-Allow-Methods`
	* `Access-Control-Allow-Headers`
	* `Access-Control-Allow-Origin`

## Security
* IAM Permissions
	* need to be attached to user/role
	* authentication: IAM
	* authorization: IAM Policy
	* use Sig v4 to use the IAM in request header
* Resource policies
	* who and what can access resources
	* allow for cross account access with IAM Security
	* allow for specific IP
	* allow for VPC endpoint
* Cognito User Pools
	* database of users
	* manage user lifecycle
	* API Gateway verifies identity automatically from Cognito
	* can use social media login
	* no custom integration needed
	* Authentication: Cognito user pools
	* Authorization: API Gateway methods
* Lambda Authorizer (Custom Authorizer)
	* Token based authorizer (bearer token) (JWT or Oauth)
	* request parameter based Lambda Authorizer (headers, query string, stage var)
	* Lambda must return an IAM policy for user; result policy is cached
	* authentication: external
	* authorizatin: lambda function (need to code)

## HTTP API vs REST API vs WebSocket
* HTTP API
	* low latency, cost effective AWS Lambda Proxy, HTTP Proxy and private integration
	* Supports OIDS and OAuth
	* built-in support for CORS
	* no usage plans
	* very cost
* REST API
	* all features
	* doesn't support Native OpenID Connect/OAuth
* WebSocket:
	* two way interactive communication between browser and server
	* server can push info to client
	* enables 'stateful' application use cases
	* for real-time applications
	* works with any kind of integrations (AWS services or HTTP endpoints)# Serverless Application Model

## Intro
* framework for developing and deploying serverless applications
* all configuration is in YAML code
* this will generate CloudFormation code from the YAML file
* Supports anything from CloudFormation: Outputs, Mappings, Parameters, Resources
* only two commands to deploy to AWS
* can use use CodeDeploy to deploy Lambda functions
* can help us run Lambda, Gateway, DynamoDB locally
* Recipe:
	* 'Transform' header indicates that it is SAM template:
		* `Transform: 'AWS::Serverless-2020-10-31'`
	* Helpers for resources
		* AWS::Serverless::Function
		* AWS::Serverless::Api
		* AWS::Serverless::SimpleTable
	* Package and Deploy:
		* CloudFormation or SAM Package
			* This will zip SAM Template, Application Code, and Swagger File and upload to S3
			* Convert SAM Template to CloudFormation Template
			* generated template will refer to S3 bucket
		* CloudFormation or SAM Deploy
			* this will generate and execute a 'change set'
			* apply the change set to the stack

## SAM Policy Templates
* list of templates to apply permissions to lambda functions
* defines what lambda functions can do
* additional resources: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-policy-templates.html
* important examples:
	* S3ReadPolicy: read only permissions to objects in S3
	* SQSPollerPolicy: allows to poll SQS Queue
	* DynamoDBCrudPolicy

## SAM and CodeDeploy
* SAM natively uses CodeDeploy to update Lambda
* define pre and post traffic hooks features to validate deployment
* automated rollback using CloudWatch Alarms# Cognito

## Intro
* used to provide application users an identity to interact with the app
* Cognito User Pools
	* sign in functionality
	* integrate with API Gateway and ALB
* Identity Pool
	* used to give users credentials to access resources
	* integrate with Cognito User Pools to use as an identity provider
* Cognito Sync
	* sync data from device to Cognito
	* is deprecated
	* replaced by AppSync

## Cognito User Pools (CUP)
* create a serverless database of users for the app
* Simple Login: username(email) or password
* Password reset
* email and phone verification
* Multi-Factory auth (MFA)
* Federated identities: Facebook, Google, SAML
* can block users if credentials compromised
* Login action sends back JWT(JSON Web Token)
* have internal database of users
* integrates with Gateway and ALB
* Lambda Triggers: Lambda function can be invoked synchronously on these triggers
	* Authentication Events:
		* Pre Authentication
		* Post Authentication
		* Pre Token Generation
	* Sign up
		* Pre sign up
		* Post confirmation
		* Migrate user
	* Messages
		* Custom messages
	* Token creation
		* Pre Token generation
* Hosted Authenticaiton UI
	* it has hosted authentication UI for signup and signin

## Cognito Identity Pools (federated identities)
* identities for users on temporary basis
* identity pool can include:
	* public providers (amazon, facebook, google, apple)
	* users in Cognito User pool
	* OpenID Connect Providers and SAML identity providers
	* developer authenticated identities (custom login server)
	* also allow for guest access
* users can access AWS services directly or through API Gateway
	* IAM policies applied are defined in Cognito
	* policies can be customized at user level
* can define Default IAM roles and also assign based on user_id
* user access can be partitioned based on policy variables
* IAM credentials are obtained by Cognito Identity Pools through STS
* Roles must have "trust" policy of Cognito Identity Pools

## User Pool vs Identity Pool
* User Pool
	* database of users for app
	* allows federated logins
	* customize UI login
	* has triggers with Lambda
	* manager username/password
* Identity Pools
	* Obtain credentials for users
	* users can login through public social, OIDC, SAML, and User Pools
	* can have guest users
	* mapped to IAM roles and policies
	* gives access to AWS services

## Cognito Sync (Deprecated)
* store preferences, config, state of app
* Cross device sync
* Offline capability 
* store data in datasets up to 1MB, up to 20 datasets to sync
* Push Sync: silently notify all devices when identity data changes
* Cognito Stream: stream data from Cognito into Kinesis
* Cognito Events: execute Lambda in response events# AWS Step Functions and AppSync

## Step Functions
* build serverless **visual workflow** to orchestrate Lambda functions
* Represent flow as JSON State Machine
* features:
	* sequencing
	* parallel
	* conditions
	* timeouts
	* error handling
* can integrate with EC2, ECS, on-premise, API Gateway
* maximum execution time 1 year
* can implement human approval
* use cases:
	* order fullfillment
	* data processing
	* web app
	* any workflow
	* additional resources: https://aws.amazon.com/step-functions/use-cases/
* any state can encounter errors:
	* state machine definition issues (eg no matching rule in choice state)
	* task failure (eg exception in lambda)
	* Transient issues (eg network partition events)
* failure in any state will fail the Step Function entirely
* Resolutions to error:
	* Retry: exponential backoff type
	* Catch: move on to next
* best practice is to add error messages
* Types of Step Functions:
	* Standard
		* max duration 1 year
		* execution start rate 2,000/second
		* state transition rate 4,000/account
		* more expensive
	* Express
		* max duration 5 minutes 
		* execution start rate 100,000/second
		* state transition rate nearly unlimited
		* cheaper

## AppSync
* managed service on GraphQL
* it makes easy for applications to get exactly the data they need
* includes combining data from one or more sources
	* NoSQL, RDBMS, HTTP APIs
	* dynamoDB, Aurora, ElasticSearch
	* Custom sources with Lambda
* it can also be used for real-time data retrieval with WebSocket or MQTT on WebSocket
* for mobile apps: local data access and data sync
* start by uploading GraphQL schema
* runs on Resolver (sources)
* Four ways to authorize apps:
	1. API_KEY
	2. AWS_IAM: users/roles/cross-account
	3. OPENID_CONNECT
	4. AMAZON_COGNITO_USER_POOLS
* for custom domain and HTTPS, use CloudFront infront of AppSync# Identify and Access Management (IAM)

## Intro
* IAM includes:
	* Users (physical person)
	* Groups (functions and teams; contains users)
	* Roles (internal usage within resources; for machines)
* 'Root' account should never be used
* Users must be created with proper permissions
* IAM is at the center of AWS
* Policies are written in JSON
* Policies interact with Users, Groups, and Roles
* IAM has global view (common for all regions)
* Permissions are governed by Policies
* MFA can be setup
* IAM has predefined policies
* Apply **Least Privilege Priciples**
* Policies will **union** for IAM and S3

## IAM Federation
* For large corporations
* can integrate company's own repository of users with IAM
* This uses SAML(example: Microsoft Active Directory)

## Tasks
* Add MFA
* Create User
* Create Group
* Additing user to group
* Remove user from group
* Setting IAM Password policy

## Manage MFA
In IAM console:
1. Click "Users" from left side menu
2. Select a user
3. Select "Security Credentials"
4. Click "Manage" beside "Assigned MFA device"
5. Select "Virtual MFA device" and "Continue"
6. Under #2, Click "Show QR Code" (Install Google Authenticator on phone first)
	* Google Authenticator: https://support.google.com/accounts/answer/1066447?co=GENIE.Platform%3DAndroid&hl=en
	* Authy: https://authy.com/
	* LastPass Authenticator: https://lastpass.com/auth/
7. Under #3, enter two consecutive code shown in the Google Authenticator app
8. Click "Assign MFA"
9. Observer confirmation and click "Close"

## Creating Users
1. Click Users
2. Add User
3. Give it a name
4. Select access type: Programmatic and Console
5. Enable "Autogenerated password" 
6. select "User must create a new password at next sign-in"
7. Next:Permission
8. Permissions can be applied based on three types:
	* Add to a group
	* Copy from existing user
	* Attach existing policies to the user
9. Next:Tags
10. Can add "Tag" and a value for the tag
11. Next:Review
12. If everything looks good, click "Create User"
13. **Important** Download the csv file or Click show in the table to see the password

## Creating Groups
1. click Groups from left
2. Set a group name and click next
3. Attach a policy (for example: AdministratorAccess) and click next
4. Click create group

## Adding Users to Group
1. click Group
2. On "Users" tab, click "Add Users to Group"
3. Select users to add to the group
4. Click "Add Users"

## IAM Password Policy
1. Click "Account Settings"
2. Change password policy
3. Select the password policies
4. Click Save

## Authorization Model
1. if there's explicit DENY, then deny
2. if there's an ALLOW, then allow
3. Deny everything else

## Dynamic Policies
* use case: allow each user a /home/<user> directory in S3: `${aws:username}`
* rather than having policies for each user, have a dynamic policy

## Types Policies
* AWS Managed Policies
	* maintained by AWS
	* good for power users and admins
	* updated by AWS in case of new services/APIs
* Customer Managed Policy
	* granular control
	* best practice
	* version controlled
	* re-usable
* Inline Policy
	* Strict one-to-one relation between policy and principal
	* policy is deleted with Principal

## AWS Directory Services
* Help manage Microsoft Active Directory
* Flavours:
	1. AWS managed MS AD
		* create AD in AWS, manage users locally
		* Supports MFA
		* can establish 'trust' connection with on-premise AD
	2. AD Connector
		* Directory Gateway (proxy) to redirect to on-premise AD
		* users are managed on-premise AD
	3. Simple AD
		* stand alone
		* AD-compatible managed directory on AWS
		* can't be joined with on-premise AD
## Notes
* It is better to assign user to groups and attach policies to groups rather than attaching policies to individual users
* Customize the account alias from IAM Dashboard# Security Token Service

## STS
* Allows to grant limited and temp access to AWS resources (up to 1 hour)
* APIs:
	* AssumeRule: within or cross account (important)
		* define IAM role
		* define which principals can access 
		* use STS to retrieve credentials and impersonate the IAM role
		* valid from 15-60 minutes
	* AssumeRoleWithSAML: return credentials for users logged with SAML
	* AssumeRoleWithWebIdentity: return credentials for users logged with facebook, google
	* GetSessionToken: for MFA(important)
		* need IAM Policy with IAM Conditions
		* in IAM Policy: `aws:MultiFactorAuthPresent:true`
		* returns:
			* AccessID
			* Secret Key
			* Session Token
			* Expiration date
	* GetFederationToken
	* GetCallerIdentity: return details about IAM user/role(important)
	* DecodeAuthorizationMessage: decode error messages when API is denied (important)# Security

## Encryption
* Encryption in flight (SSL)
	* encrypt data before sending
	* use SSL certificates
	* ensures protection from Man-in-the-middle attack
* Server-side encryption at rest
	* data encrypted at server
	* decrypted before sending
	* encryption and decryption with a key
	* the key is managed by Key Management System (KMS)
* Client-side encryption
	* encrypted by the client
	* never decrypted by the server
	* decrypted by the receiving client
	* can use Envelope Encyption

## Key Management Service (KMS)
* in AWS, encryption is synonymous to KMS
* AWS manages keys for us
* integrated with IAM
* can be used with CLI/SDK
* when exceed request quota get a ThrottlingException + exponential backoff
* cryptographic operations share a single quota, including requests made by AWS
* for GenerateDataKey, better use DEK Caching
* can also request quota increase by API/Ticket
* Customer Master Keys (CMK) types:
	1. Symmetric (AES-256 bit Keys)
		* single key to encrypt and decrypt
		* first offering of KMS
		* many services use this under the hood
		* never get access to unencypted key (must call KMS API)
	2. Asymmetric (RSA and ECC Key pairs)
		* two keys: Public key (encrypt) and Private Key (decrypt)
		* SSL uses this

## Symmetric Keys
* able to manage keys and policies:
	* create
	* rotation policies
	* disable
	* enable
* audit key usage with CloudTrail
* Three types of CMK:
	1. AWS managed service Default CMK (Free)
	2. User created Key: $1/month
	3. User imported (must be 256bit symmetric): $1/month
* pay  $0.03/10000 API calls
* user should never be able to retrieve the key
* encypted secrets should be stored in environment variables
* KMS can encrypt up to 4KB per call
* anything over 4KB, need Envelope Encryption
* Giving access to KMS to someone:
	* Key policy allows
	* IAM policy allows
* KMS Keys are bound to a region

## KMS Key Policies
* control access to KMS keys
* cannot access without Policies
* Default KMS Key policy:
	* created if a policy is not specified
	* complete access to the key to the root user
	* give access to IAM policies to KMS Key
* Custom KMS Key Policy:
	* define users, roles that can access the KMS Key
	* define who can administer
	* useful for cross-account access of KMS Keys

## Envelope Encyption
* to encrypt over 4KB
* API for this: GenerateDataKey
* encryption happens client-side
* can be implemented using Encryption SDK/CLI
* also has Data Key Caching (reuse data key)

## SSM Parameter Store
* secure storage for config and secrets
* serverless, scalable, durable, easy SDK
* optional encryption using KMS
* versioning of config/secrets
* config management using path & IAM
* notification with CloudWatch Events
* integration with CloudFormation
* Tiers:
	* Standard
		* total parameter: 10,000
		* max size of value: 4KB
		* parameter policies not available
		* storage pricing free
		* standard throughput free but $0.05 per 10,000 over 1000 transactions per second
	* Advanced
		* total parameter: 100,000
		* max size of value: 8KB
		* parameter policies available
		* storage not free
		* throughput $0.05 per 10,000
* Parameter policies allow TTL. can assign multiple policies at a time

## AWS Secrets Manager
* stores secrets
* can force rotation after X days
* automate generation of secrets on rotation using Lambda
* integrate with RDS
* encrypted using KMS
* mostly meant for RDS integration

## SSM Parameter Store vs Secrets Manager
* Secrets Manager:
	* expensive
	* auto rotation
	* KMS mandatory
	* integration with RDS
	* integation with CloudFormation
* SSM Parameter Store
	* less expensive
	* no secret rotation
	* KMS optional
	* integration with CloudFormation
	* can pull Secrets Manager secret using Parameter Store API

## CloudWatch Logs Integration
* can encrypt CloudWatch logs with KMS keys
* encryption is enabled at log group level by associating a CMK with a log group, either when creating or after. has to use CloudWatch Logs APIs:
	* associate-kms-key (for existing log group)
	* create-log-group (if log group doesn't exist)

## CodeBuild Security
* to access resources in VPC, specify VPC config for CodeBuild
* don't store them as plaintext in environment variables
* suggested way to store environment variables:
	* refer to parameter store parameters
	* refer to secrets manager secrets# Amazon S3

## Buckets
* allows people store object(files) in buckets (directories)
* globally unique name
* defined at regional level
* Naming conventions:
	* no uppercase
	* no underscore
	* 3-63 characters
	* not an ip
	* start with lower case letter or number
* objects have key. the key is FULL path of that object inside the bucket. The key can have Prefix (path inside the bucket) and Object name
* there's no concept of directories inside bucket
* object values are content of the body:
	* max object size 5TB
	* cant upload more than 5GB. Must upload using "multi part upload"
* Objects can have metadata (key-value pairs upto 10)
* Objecsts can also have tags and version ID
* The bucket is private by default. So the objects in it will not be accessible by public.

## Versioning
* files in S3 can be versioned
* enabled at bucket level
* if some file exists, it will not overwrite rather create versions
* it is a best practice
	* protect against unintended deletion
	* easy roll back to previous versions
* files that are not versioned before enabling it will have "null" as version
* suspending doesn't delete versions

## Encryption
* There are 4 methods of encryption
	1. SSE-S3: uses keys handled and managed by AWS
		* sever side encryption
		* AES-256 encryption
		* must set header: "x-amz-server-side-encryption":"AES256"
		* because of the header, when an object is uploaded to S3, it will know how to encrypt it and process accordingly
	2. SSE-KMS: uses AWS Key Management Service
		* gives control on user
		* give audit trail
		* server side encrytion
		* must set header: "x-amz-server-side-encryption":"aws:kms"
		* the header instructs AWS to encryption
		* throttling can happen because of KSM limit
	3. SSE-C: client manages their own keys
		* AWS S3 doesn't store the ecryption keys
		* HTTPS must be used
		* encyrption key must be provided in request header
		* to retrieve the object, the key must also be provided
	4. Client Side Encryption
		* encrypt the object before uploading
		* clients must decrypt the data themselves
		* Amazon S3 Encryption Client can provide the help
* Encryption in transit:
	* Amazon S3 exposes
		* HTTP endpoint: non encrypted
		* HTTPS endpoint: encryption in flight
	* HTTPS recommended and default, which is mandatory for SSE-C
	* also called SSL/TLS
	* to enforce SSL, Create bucket policy with DENY on condition: `aws:SecureTransport=false  `

## S3 Security
* User based
	* IAM policy: which API calls should be allowed for a specific user
* Resource based:
	* Bucket Policy: bucket wide rules from S3 console, allows cross account access
		* https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html
		* https://aws.amazon.com/blogs/security/how-to-prevent-uploads-of-unencrypted-objects-to-amazon-s3/
		* Bucket Policy Generator: https://awspolicygen.s3.amazonaws.com/policygen.html
		* JSON based
		* Resources: buckets and objects
		* Actions: set of API to Allow/Deny
		* Effect: Allow/Deny
		* Principal: account or user to appy the policy to
		* used to grant public access
		* force objects to be encrypted at upload
		* grant access to another account
	* Object ACL: finer than bucket policy
	* Bucket ACL: less common
	* Bucket Settings for block Public Access (four types):
		* new access control lists (ACLs)
		* any access control lists (ACLs)
		* new public bucket or access point policies
		* any public bucket or access point policies
		* there four are used so the bucket will never be public and can be set at account level
* An user can access an object if IAM permissions allow or the resource policy allows; and there's no explicit DENY
* Networking: Support VPC Endpoints
* Loggin and Audit: 
	* S3 Access Logs can be stored in Bucket
	* API calls can be logged in CloudTrail
* User Security:
	* MFA Delete: MFA can be required in versioned buckets to delete objects
	* Presigned URL: valid for a limited time. use case: premium video service for logged in users

## S3 Websites
* S3 can host static websites and have them accessed from internet
* the website URL will be:
	* <bucket-name>.s3-website-<aws-region>.amazonaws.com, or
	* <bucket-name>.s3-website.<aws-region>.amazonaws.com
* if website is enabled but the bucket policy is not allowed for public reads, then the user will get a 403 (Forbidden) error.

## Cross Origin Resource Sharing (CORS)
* origin is a scheme (protocol), host(domain), and port
* this means we want to get resources from a different origin
* this is a browser based security to allow requests to other origins while visiting the main origin
* The request will not be fulfilled unless the other origin allows 
* CORS Headers: Access-Control-Allow-Origin
* in case of CORS, the web browser makes a "Preflight Request" to the other origin and the other origin responds with the original hostname and methods that can be used (GET, PUT, DELETE, for example). After this, web browser can do the actual request.

## S3 CORS
* to do cross-origin requests, we need to enable correct CORS headers
* we can allow for specific origin or all origin (`*`)

## S3 - Consistency Model
* read after write consistency for PUTS of new object (Eventually Consistent)
	* we can retrieve an object as soon as we write it: PUT 200 lead to GET 200
	* if we do a GET before PUT we will get a 404. If we do another GET after the PUT, even though the object exists, we would get a 404. 
* Eventually consistency for DELETES and PUTS of existing objects
	* if we read an object after updating, we might get the olde version. PUT 200 => PUT 200 => GET 200. May need to wait for some time
	* if we delete an object, we might still be able to retrieve it for a short time: DELETE 200 => GET 200
* There's not way to get "Strong Consistency"

## S3 MFA - Delete
* need to enable Versioning on the S3 bucket
* Need MFA to 
	* permanently delete an object version
	* suspend versioning
* Don't need MFA to
	* enable versioning
	* listing deleted verion
* Only Root can enable/delete MFA-Delete
* this can be enabled only through CLI

## S3 Access Logs
* log all access to S3 Buckets
* any request made to S3, from any account, authorized or denied will be logged into another S3 Bucket
* The data can be analyzed using data analysis tools or Amazon Athena
* Log Format: https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html
* Don't set the logging bucket to the bucket being monitored
* This option can be found in Properties of the bucket called "Server Access Logging"

## S3 Replication (CRR & SRR)
* Asynchronous replication to another bucket in same region or not
* CRR = Cross Region Replication
* SRR = Same Region Replication
* Must enable versioning in source and destination
* Bucekts can be in different accounts
* must give proper IAM permission to S3
* CRR use case: compliance, lower latency access, replicaiton across accounts
* SRR use case: log aggregation, live replication between prod and test
* Only new objects will be replicated after enabling it
* delete operations are not replicated (we can enable marker replicaiton but theres no way to replicate permanent delete)
* no chaining of replication
* https://docs.aws.amazon.com/AmazonS3/latest/dev/replication.html

## S3 Pre-signed URLs
* can be generated using SDK/CLI
	* download: better to use CLI
	* upload: must use SDK
* valid for 3600 seconds, can be changed with `--expires-in <time>` argument
* users given the url inherit the permissions of the creator
* Use case:
	* only allow logged in users to download premium content
	* allow an ever changing list of users to download a file
	* allow temporarily a user to upload a file to location
* `aws s3 presign s3://mybucket/myobject --region my-region --expires-in 300`

## S3 Storage Classes
* Amazon S3 Standard - General Purpose
	* high durability (11 9's) of objecs multiple AZ
	* sustain 2 concurrent facility failures
	* big data, mobile/gaming apps, content distribution
* Amazon S3 Standard - Infrequent Access (IA)
	* less frequent but rapid access
	* high durability
	* lower cost compared to General purpose
	* sustain 2 concurrent failures
	* disaster recovery, backups
* Amazon S3 One Zone - Infrequent Access
	* stored in signle AZ; data lost if AZ is destroyed
	* low latency and high throughput
	* supports SSL for data transit and encryption
	* less availability (99.5%)
	* secondary backup, data that can be recreated
* Amazon S3 Intelligent Tiering
	* high throughput and low latency
	* small monthly monitoring fee and auto-tiering fee
	* automatically moves objects between two access tiers bese on changing access patterns
	* high durability
	* resilient against events affecting AZ
	* 99.9% availability over a given year
* Amazon Glacier
	* cold archieve
	* low cost object storage for backup/archiving
	* data is retained for longer term
	* alternative to on-premise magnetic tape
	* very high durability
	* low cost per storage per month + retrival cost
	* each item in Glacier is called "Archive" (upto 40TB)
	* Archives are stored in Valuts
	* Three retrieval options:
		1. Expedited (1-5 minutes)
		2. Standard (3-5 hours)
		3. Bulk (5 - 12 hours)
	* Minimum storage duration 90 days
* Amazon Gracier Deep Archive
	* for longer term storage
	* cheaper
	* two retrieval options:
		1. Standard (12 hours)
		2. Bulk (48 hours)
	* minimum storage duration 180 days
* Amazon S3 Reduced Redundancy Storage (Deprecaed)
![Storage Classes Comparison](https://jayendrapatil.com/wp-content/uploads/2016/03/S3-Storage-Classes-Performance.png)

## Moving Between Storage classes
* objects can be moved between storage classes
* moving objects can be automated using lifecycle configuration
* Transition Actions can be defined, which is when to move an object from one class to another
* Expiration Actions can be used to delete log files or old versions of a file. It can also be used to clean incomplete multi-part uploads
* Rules can be applied to specific prefix (.mp3) and file tag (finance)

## S3 Performance
* Baseline performance
	* auto scales up to high request rates (latency 100-200ms)
	* applications can achieve at least 3500 PUT/COPY/POST/DELETE and 5500 GET/HEAD requests per second per prefix in a bucket
	* in `bucket/folder1/sub1/file`, `bucket/folder1/sub1/` is the prefix
* KMS Limitation
	* performance may be affected KMS limits, if SSE-KMS is used
	* when uploading, GenerateDataKey KMS API is called
	* When downloading, Decrypt KMS API
	* KMS quota is based on region, ranging from 5500 to 30000 
	* quota request can't be increased
* Multi-part upload
	* recommended for files over 100MB; must for files over 5GB
	* Can help parallelize and incease the transfer speed
* S3 Transfer Acceleraiton (upload only)
	* increase transfer speed by first transferring file to a Edge location and forwarding that to S3 Bucket
	* compatibe with multi-part upload
* S3 Byte-Range Fetches
	* parallelize GETs by specifying byte ranges
	* beter resilience in case of failures
	* can speed up downloads
	* only retrieve only partial data (head of the file)

## S3 Select and Glacier Select
* retrieve less data using SQL by performing server side filtering
* can filter by rows and columns
* less network traffic, less CPU cost at client-side

## S3 Event Notifications
* Notification and actions can be based on:
	* actions/notification: creation, deletion, restoration,
	* type of object: .jpg file
	* generate thumbnails 
	* Targets:
		1. SNS (Simple Notification Service): send notification to emails
		2. SQS (Simple Queue Service): send notifiaction to a queue
		3. Lambda Function: generate some custom code
	* can create as many notifications as we want
	* if two writes on single non-versioned object is done at the same time, one notification COULD be generated

## AWS Athena
* serverless service to perform analytics agains S3 files
* SQL to query files
* JDBC/ODBC drivers to connect
* charged per query and amount of data scanned
* support CSV, JSON, ORC, Avro, Parquet (built on Presto)
* Use cases: Business intelligence/analytics/reporting, VPC flow logs, ELB logs, CloudTrail
* Additional Resources: https://aws.amazon.com/premiumsupport/knowledge-center/analyze-logs-athena/

## S3 Object Lock and Glacier Vault Lock
* S3 Object Lock
	* adopt a WORM (Write Once, Read Many) model
	* block object version deletion for a specified time
* Glacier Vault Lock
	* same WORM model
	* lock the policy for future edits and can't be changed
	* helpful for compliance and data retention# Miscellaneous Services

## Simple Email Service
* send email to people
* using:
	*  SMTP
	* AWS SDK
* receive email integrated with:
	* S3
	* SNS
	* Lambda
* need IAM permission

## Database Summary
* RDS: relational database; good for OLTP (on-line transaction processing)
* DynamoDB: noSQL, managed, key-value, serverless
* ElastiCache: in-memory, redis/memcached, for caching
* Redshift: OLAP (on-line Analytic Processing); data warehousing/data lake
* Neptune: Graph database
* DMS: Database migration service
* DocumentDB: managed MongoDB for AWS

## AWS Certificate Manager (ACM)
* host public SSL certifcates in AWS
* two ways to manage:
	* buy and upload using CLI
	* Let ACM provision and renew public SSL (for free)
* will load them:
	* Load Balancer including EB created
	* CloudFront
	* APIs on API gateways* Console address: https://hussain-aws-course.signin.aws.amazon.com/console# Terms
