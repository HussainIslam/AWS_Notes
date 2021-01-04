# Integration and Messaging

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
