# EC2

## Intro
* It can:
	* rent virtual machines (EC2)
	* Store data on virtual drives (EBS)
	* Distribute load across machines (ELB) 
	* scaling services (ASG)
* 

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
