# Route 53

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

## 