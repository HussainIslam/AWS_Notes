# Elastic Container Registry (ECS)

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
	* used in association with cluster to determine the infrastructure that a task runs on