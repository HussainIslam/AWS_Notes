# Continuous Integration, Continuous Deployment (CICD)

## Intro
* We would like to push our code in a repository and have it deployed on AWS:
	* automatically
	* the right way
	* tested before deploying
	* with different stages (dev/test/preprod/prod)
	* manual approval, if neeeded
* Parts:
	* CodeCommit: store code
	* CodePipeline: automating pipeline from code to ElasticBeanstalk
	* CodeBuild: building and testin code
	* CodeDeploy: deploying to EC2 fleet
* Continuous Integration:
	* devs push code to repository often (github/codecommit/bitbucket)
	* testing/build server as soon as it's pushed (codeBuild/Jenkins CI)
	* devs get feedback from build server
	* find bugs early, fix bugs
	* deliver faster as code is tested
	* deploy often
* Continuous Delivery:
	* ensure software can be released reliably
	* ensure deployments happen often and quick
	* shift away from "one release every 3 months"
	* Options: CodeDeploy, Jenkins CD, Spinnaker
* Technology Stack for CICD
	1. Code: CodeCommit/GitHub
	2. Build: CodeBuild, Jenkins CI, 
	3. Test: CodeBuild, Jenkins CI, 
	4. Deploy: Beanstalk, CodeDeploy
	5. Provision: Beanstalk, CloudFormation
	6. Orchestration: CodePipeline

## CodeCommit
* version control
* lets collaborate
* make sure the code is backed-up
* traits:
	* private Git repositories
	* no size limit
	* fully managed, highly available
	* increased security and compliance
	* secure (encryption, access control)
	* integrated with other 3rd party tools
* Authentication:
	* SSH Keys: configured in IAM Console
	* HTTPS: AWS CLI Authentication Helper or Generating HTTPS Credentials
	* MFA: extra safety
* Authorization: IAM Policies users/roles
* Encryption:
	* Encryption at-rest: KMS
	* Encryption in-transit: SSH/HTTPS
* Cross Account Access:
	* AWS STS (with AssumeRole API)
* It can trigger notifications using 
	* SNS (Simple Notification Service)
	* AWS Lambda 
	* AWS CloudWatch Event Rules
* Use cases of SNS/Lambda:
	* Deletation of branches
	* pushes in master branch
	* notify external build system
	* AWS Lambda to perform codebase analysis (credential got committed)
* Use cases of CloudWatch Event Rules:
	* Trigger of pull requests(create/update/delete/commented)
	* Commit comment events
	* goes into SNS topic

## CodePipeline
* For continuous delivery
* visual workflow
* Source: github/codecommit/S3
* build: codeBuild/Jenkins
* Load testing: 3rd party tools
* Deploy: CodeDeploy/BeanStalk/CloudFromation/ECS
* Made of Stages: sequential/parallel actions; build/test/deploy/load test; manual approval can be added
* must have an IAM role attached to it
* each stage can create artifacts; can be stored in S3 and passed to the next stage
* state changes generate CloudWatch Events, which in turn generate SNS notifications; 
	* create event for failed pipeline
	* create event for cancelled stages
* API Calls to it can be audited using CloudTrail
* If pipeline can't perform an action, make sure the permission of IAM Service Role
* as stage can have multipe action groups (sqeuantial or parallel)

## CodeBuild
* full managed build service
* alternative to Jenkins
* continuous scaling (no servers to manage; no build queue)
* pay for usage
* leverages docker under the hood
* can add capabilities using own base Docker images
* security: 
	* integratin with KMS for encryption of build artifacts
	* IAM for build permissions
	* VPC for network security
	* CloudTrail for API call logging
* source code from gitHub/CodeCommit/CodePipeline/S3
* Build instructions defined by `buildspec.yml` file
	* at the root of code
	* define environment variables:
		* plaintext variables
		* security secrets (SSM Parameters)
	* Phases:
		* Install dependencies
		* Prebuild: commands to execute for build
		* Build
		* Post build: finishing touch
	* At the end get Artifacts (KMS Encryption as well)
	* Cache: Files to cache; usually intermediate artifacts
* Outputs log to S3 and CloudWatch Logs
* metrics to monitor metrics
* cloudwatch events or AWS Lambda as a glue SNS Notification
* can be reproduced locally to debug (CodeBuild Agent)
* can be defined within CodePipeline or CodeBuild
* By default CodeBuild containers are launched outside VPC. To access need to specify:
	* VPC ID
	* Subnet ID
	* Security Group ID

## CodeDeploy
* helps to deploy app to many EC2 instances but not managed by Beanstalk
* Several ways to handle deployment: Ansible, Terraform, Chef, Puppet
* Managed service
* Steps:
	* EC2/Local machine must run CodeDeploy Agent
	* Agent will continuously poll CodeDeploy to work
	* CodeDeploy will send appspec.yml (at the root of project)
	* App is pulled from GitHub/S3
	* EC2 run the deployment instructions
	* CodeDeploy Agent will report success/failure
* EC2 Instances are grouped by deployment group (dev/test/prod) but have flexibility
* can be integrated with CodePipeline and use artifact
* can use existing setup tools, works with any app, auto scaling integration
* Can do Blue/Green deployment (not on premise)
* support AWS Lambda
* only deploys; doesn't provision resources
* Primary Concepts:
	* Application: unique name
	* Compute platform: EC2/on-premise
	* Deployment configuration: Deployment rules for success/failure
	* Deployment groups: group tagged instances (allow to deploy gradually)
	* Deployment type: In-place or Blue/Green
	* IAM Profile: required to pull code from S3/Github
	* Application: Code + appspec.yml file
	* Service role: role fo CodeDeploy to perform actions
	* Target revision: target version of upgrade of app
* appspec.yml:
	* File section: how to source/copy code
	* Hooks: what to do to deploy code (can have timeouts)
		* ApplicationStop: Stops the current version of app
		* DownloadBundle: download the new app
		* BeforeInstall: prep jobs before install
		* Install
		* AfterInstall: clean up after install
		* ApplicationStat: how to start app
		* ValidateService: validate the application is working
		* BeforeAllowTraffic
		* AllowTraffic
		* AfterAllowTraffic
	* Configs:
		* One at a time
		* Half at a time
		* All at once
		* Custom
	* Failures:
		* instances stay at failed state
		* new deployments will be deployed to failed states first
		* Rollback: redploy or enable automated rollback
	* Deployment targets:
		* set of EC2 instances with tags
		* Directly to ASG
			* can have in-place or Blue/Green (need ALB)
		* mix of Tags and ASG
		* Customization in scripst with DEPLOYMENT_GROUP_NAME environment variable

## CodeStar
* integrated solution to regroup all the CICD services
* wrapper around everything to EC2, Lambda, Beanstalk
* Issue tracking with Jira and Github Issues
* Ability to integrate with Cloud9 as IDE
* One dashboard for all components
* free service, pay for underlying services
* limited customization