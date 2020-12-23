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
