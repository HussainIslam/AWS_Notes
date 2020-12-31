# Elastic Beanstalk

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
