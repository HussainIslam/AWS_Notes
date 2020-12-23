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
* Preparing EBS for use: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html 