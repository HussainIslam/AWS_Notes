# AWS Basic Concepts

[Reference](https://aws.amazon.com/about-aws/global-infrastructure/)

## AWS Regions
* AWS has divided the world in "Regions"
* Naming convention: us-east-1, eu-west-3
* "Regions" have data centers
* Services are region-specific

## AWS Availability Zones (AZ)
* Each region has many (AZ) (ranging 3-6)
* AZs are one or more discrete data centers with redundant power, networking, and connectivity
* In essence, they are isolated from disasters
* The AZs are connected through high-bandwidth and ultra-low latency networking
* Naming:
	* AZs are referred a/b/c and so on after the "Regions"
	* ap-southwest-2*a*
	* ap-southwest-2*b*
	* ap-southwest-2*c*

## Scalability and High Availability
* Scalability: application/system can handle greater loads by adapting
* Two kinds of scalability:
	1. Vertical scalability
		* increase the size of instance
		* t2.micro to t2.large
		* usually for non-distributed system
		* Example: database - RDS, ElastiCache
	2. Horizontal Scalability (also called elasticity). This can be scale in (increase) or scale out (decrease)
		* increase the number of instance
		* implies distributed system
		* very common for web apps
* High Availability: running app in at least 2 data centers/AZ. The goal is to survive data center loss.
* HA can be two types:
	1. Passive: RDS Multi AZ
	2. Active: Horizontal scaling
