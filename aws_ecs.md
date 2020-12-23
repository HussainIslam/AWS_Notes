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
	 