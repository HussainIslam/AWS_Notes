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
* 
