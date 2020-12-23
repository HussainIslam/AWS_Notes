# Virtual Private Cloud (VPC)

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
* Data Subnet: RDS, ElatiCache