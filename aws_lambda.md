# AWS Lambda

## Intro
* Serverless is when you don't have to manager server; just deploy and it works. Initially, serverless ment Function as a Service (FaaS)
* Serverless Technologies include:
	* AWS Lambda
	* DynamoDB
	* AWS Congnito
	* AWS API Gateway
	* Amazon S3
	* AWS SNS
	* AWS SQS
	* AWS Kinesis Data Firehose
	* Aurora Serverless
	* Step Functions
	* Fargate

## AWS Lambda
* virtual functions; no servers to manage
* limited by time - short executions
* run on demand
* billed for running 
* scaling automated
* Benefits:
	* easy pricing
	* pay per request and compute time
	* free tier of 1 million AWS Lambda Requests and 400,000 GBs of compute time
	* integrated with AWS suites of services
	* integrated with many programming languages
	* easy monitoring through AWS CloudWatch
	* can get upto 3GB of RAM per function
	* updating RAM will update CPU and Network
* Supported Languages:
	* Node
	* python
	* Java
	* C#/(.Net Core)
	* Golang
	* C#/Powershell
	* Ruby
	* Custom Runtime API (community supported, ex: Rust)
* **Docker doesn't run on lambda**
* Pricing:
	* pay per calls
		* first 1 million requests are free
		* after that per 1 million requests cost $0.20
	* Pay per Duration (incremnet of 100ms):
		* 400,000 GBs of compute time per month is free
		* 400,000 seconds of execution for 1GB RAM
		* after that $1 for 600,000 GBs

## Lambda - Synchronous Invocation
* done through: CLI, SDK, API Gateway, ALB
* results are returned right away
* client will be waiting for the response
* error handling must happen client side (retires, exponential backoff)
* services that are synchronous:
	* use invoked:
		* ELB
		* API Gateway
		* CloudFront(Lambda@Edge)
		* S3 Batch
	* Service invoked:
		* Cognito
		* Step Functions
		* Lex
		* Alexa
		* Kinesis Data Firehose

## Lambda Integration with ALB
* to expose lambda function to HTTP(S) endpoint: 
	* can use Application Load Balancer (or API Gateway); 
	* Lambda must be registered in Target Group
	* client will send requests to ALB in HTTP
	* ALB will forward the request to Lambda and receive response from it
	* Finally, forward the response to client
	* HTTP request gets converted into JSON
	* query string paramters and headers are converted to key-value pairs
	* Also, ALB convers the response from JSON to HTTP
	* Multi-Header Values can be enabled at ALB settings;
	* Multi-header values in query string paramter are converted to arrays within the Lambda event and response objects

## Lambda@Edge
* another synchronous deployment of Lambda
* deploy Lambda functions not in a specific region but alongside each region with CloudFront
* Lambda will be deployed globally
* Customize CDN content
* Pay for use
* Lambda can be used to change CloudFront request and responses
	* modify reqeusts from a viewer (viewer request)
	* modify requests to origin (origin request)
	* modify response from origin (origin response)
	* modify response to the viewer (viewer response)
	* can generate responses without even sending requests to origin
* we can easily make global applications like:
	1. static website in S3
	2. User visits website and JS in 
	3. website does API request to CloudFront
	4. CloudFront invokes Lambda@Edge function
	5. Lambda@Edge queries data from DynamoDB
* Benefits:
	* security and privacy
	* Dynamic web application at edge
	* SEO
	* intelligently route across origins and Data centers
	* bot mitigation at edge
	* real-time image transformation
	* A/B Testing
	* User authentication and authorization
	* user prioritization
	* user tracking and analytics

## Lambda - Asynchronous Invocations
* asynchronous invocation can be done using:
	* S3
	* SNS
	* CloudWatch Events or EventBridge
	* CodeCommit
	* CodePipeline
	* Simple Email Service
	* CloudFormation
	* AWS Config
	* AWS IoT
	* AWS IoT Events
* S3 Bucket may have New File Event which will place place in Event Queue
* Lambda Function will be reading and processing from that event queue
* if something goes wrong, Lambda will retry (3 times)
	* 1st one right away
	* 2nd one 1 min after 1st one
	* 3rd one 2 min after 2nd one
* because of the retries processing need to be idempotent (output the same result every time)
* during reties, may see duplicate log entries in CloudWatch
* can define a DLQ (Dead Letter Queue) - SNS or SQS - for failed processing (need IAM permission)

## Lambda with CloudWatch Events or EventBridge
* there are two ways:
	1. serverless cron or Rate: trigger every one hour to invoke Lambda
	2. CodePipeline Rule: on state changes invoke Lambda

## Lambda with S3 Event Notification
* get notified when something happens in S3
* notification can have three patterns:
	1. S3 to SNS to Fan out to SQS
	2. S3 to SQS to Lambda
	3. S3 to Lambda to DLQ (SQS)
* for notification of every successful write, enable versioning

## Lambda - Event Source Mapping
* applies to:
	* Kinesis Data Streams
	* SQS and SQS FIFO Queue
	* DynamoDB Streams
* Lambda need to messages/data from these services
* Lambda invoked synchronously
* Event Source Mapping is going to poll from sources and will get batches. Then it will invoke Lambda with Event batch.
* Categories:
	1. Streams
		* Kinesis Streams and DynamoDB Streams
		* event source mapping creates iterator for each shard
		* Processes items in order at shard level
		* Start from beginning or a specific timestamp
		* items are not removed from stream
		* low traffic: use batch window for accumulate records before processing
		* can process multiple batches in parallel
		* one lambda invocation per stream shard
		* upto 10 batches per shard
		* by default, if a batch process fails, it is reprocessed until success or items expire
		* to ensure in-order processing, the affeced shard is paused until error is resolved
		* event source mapping can be configured to:
			* discard old events
			* restrict number of retries
			* split the batch on error
		* Discarded events can go to a Destination
	2. Queues
		* SQS and SQS FIFO
		* polled by Event source mapping using Long Polling
		* can specify batch size (1-10 messages)
		* Standard queue: lambda adds 60 more instances per minute to scale up; upto 1000 batches of messages processed simultaneously
		* FIFI queue: messae with same groupID will be processed in order; scales up to the number of active message groups
		* Recommended to set the queue visibility timeout to 6 times of Lambda Function timeout
		* To use DLQ, setup on SQS Queue not on Lambda (DLQ for Lambda only for async invocation)
		* Lambda Destition can also be used
		* for SQS FIFO, Lambda scales up to number of active message groups
		* not in-order for standard queue
		* Lambda scales up to process standard queue ASAP
		* on error, batches are returned to queue as individual items and may be processed in different groups
		* event source mapping may receive an item twice even without error (prevented by idempotency)
		* deleted from the queue after processing
		* source queue can be configured to send items to DLQ

## Lambda Destinations
* can configure to send results to a destination
* asynchronous invocation: destination can be used for successful or failed events
	* services can used:
		* SQS
		* SNS
		* Lambda
		* EventBridge Bus
	* recommendation is to use Destination instead of DLQ (both can be used at the same time)
* Event Source Mapping: for discarded event batches
	* services:
		* SQS
		* SNS
	* with SQS, both DLQ or Destination can be setup

## Lambda Execution Role (IAM Role)
* role must be attached to services
* there are many managed policies for Lambda
* for event source mapping, lambda uses execution role to read event data
* Best practice: create one Lambda Execution role per function
* If other resources use Lambda, then we need to use Resource based policies to let other services use Lambda resources
* IAM Principal can access Lambda:
	* if IAM policy attached to principal authorizes it
	* or if we have resource based policy

## Lambda Environmet Variables
* these are key-value pairs in string format
* help adjust function without updating code
* these are available to code as well
* lambda adds its own variables as well
* these can be used to store secrets
* secrets can be encrypted by Lambda Service Key or own CMK

## Lambda Loggin & Monitoring
* integrated with CloudWatch Logs to store Lambda execution logs with proper IAM policy
* Lambda metricsa re displayed in CloudWatch Metrics: 
	* invocations, 
	* durations, 
	* concurrent executions
	* Error count
	* Success rates
	* Throttles
	* Async delivery failures
	* Iterator age (Kinesis and DynamoDB Streams)
* Tracking can be done with X-Ray, called Active X-Ray
* runs X-Ray Daemon automatically
* use X-Ray SDK in code
* ensure Lambda has correct IAM (AWSXRayDaemonWriteAccess)
* There are environment variables to communicate with X-Ray:
	* `_X_AMAZN_TRACE_ID`: contains the tracing header
	* `AWS_XRAY_CONTEXT_MISSING`: by default, `LOG_ERROR`
	* `AWS_XRAY_DAEMON_ADDRESS`: X-Ray daemon `IP:PORT` (most important)

## Lambda in VPC
* by default, launched outside default VPC. so, can' access resources within the default VPC
* to deploy within VPC, we need to define VPC ID, subnets, and secuirty groups. Lambda will create an ENI in the subnets. 
* to create this ENI, Lambda will require `AWSLambdaVPCAccessExecutionRole`
* by default, Lambda inside default VPC doesn't have internet
* deploying Lambda in public subnet will NOT give it internet or public IP
* to give it internet access, we need to deploy in private subnet and give it NAT Gateway/Instance (configed through route tables)

## Lambda Performance
* Configuration
	* RAM: default 128MB; max 10240MB; increases vCPU with RAM; 1,792MB equal to 1 full vCPU
	* to handle computation heavy application (higher CPU), increase RAM
	* default timeout of 3 seconds; max 900 seconds (15 mintutes);
* execution context
	* temporary runtime environment that initializes external dependencies of our code
	* great for database connections, HTTP clients, SDK clients
	* maintained for some time in anticipation of another invocation (reused)
	* includes `/tmp` directory
* if something needs to be done again and again, its better to put it outside the function handler
* temporary files:
	* use `/tmp` space
	* good to download big file to work or if lambda needs disk space to perform something
	* max size is 512MB
	* the content remains when the context is frozen, providing transient cache to be used by multiple invocations;
	* this could be helpfult to checkpoint work
	* for permanent persistence, use S3

## Lambda Concurrency and Throttling
* concurrency limit of 1000 executions
* "Reserved Concurrency" at functional level can be set to restrict number of concurrent executions
* each invocation over this limit will trigger a "Throttle"
* Throttle behaviors:
	* synchronous invocation: return ThrottleError - 429
	* Asynchronous invocation: retry automatically and go to DLQ
* for over 1000 limit, open a support ticket
* issues:
	* if not reserve concurrency is set, one function may get more executions and other functions may be chocked
	* if not enough concurrency available for async invocations, additional reqeusts will be throttled. 
	* async events: Lambda will return the event to queue (throttling error: 429, and system error: 500s) and try to run it again for upto 6 hours; retry time increases from 1sec to max 5 minutes (exponential backoff)
* Cold starts and Provisioned Concurrency:
	* when new Lambda instance is created, the code needs to be loaded and code outside the handler need to be run first (init)
	* if the 'init' is large, this can take some time
	* so, the first request served have higher latency than the rest (cold start)
	* This can be overcome by Provisioned Concurrency
	* concurrency is allocated before the function is invoked
	* Application auto scaling can manage concurrency (schedule or target utilization)
	* additional resource: https://aws.amazon.com/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/

## Function Dependencies
* if the function depends on external libraries (AWS X-Ray SDK, Database Clients), we will need to install the packages along code and **zip it together**
* example:
	* node.js: npm and 'node_modules' directory
	* python: use `pip --target` options
	* java: include .jar files
* upload the zip file to Lambda, if less than 50MB; otherwise S3 first and reference in code
* native libraries work; need to be compiled on Amazon Linux
* AWS SDK comes default with all Lambda Functions

## Lambda and CloudFormation
* Inline
	* very simple use case
	* within our CloudFormation template
	* need to use the Code.ZipFile property of the template
	* can't include function dependencies
* Through S3
	* store the Lambda Zip in S3
	* refer the zip location in CloudFormation code:
		* S3Bucket
		* S3Key (full path to zip)
		* S3ObjectVersion (if versioned bucket)
	* need to manually update version in Lambda

## Lambda Layers
* use cases:
	* enable to create Custom Runtimes; C++ and Rust
	* Externalize dependencies to re-use them
* The way is to create a layer for Lambda. The layers can be referenced from the function. This way we don't need to repackage everthing each time we update the version of app
* Additional reading: https://aws.amazon.com/blogs/aws/new-for-aws-lambda-use-any-programming-language-and-share-common-components/

## Lambda Versions and Alias
* Versions: 
	* initially working with Lambda Function, the version is $LATEST (this is mutable)
	* when we're ready to publish a Lambda function, we can create a version
	* versions are immutable (cant change code or environment variables)
	* versions will have increasing number
	* versions get their own ARN (Amazon Resource Number)
	* Version = Code + configuration
	* each version of lambda can be accessed
* Aliases:
	* pointers to versions
	* we can define dev/test/prod aliases and have them point at different lambda versions
	* they are mutable
	* can be used with Blue/Green deployment
	* enable stable configuration of event triggers/destinations
	* Aliases have their own ARNs
	* Aliases can't point to other aliases

## Lambda and CodeDeploy
* can help automate traffic shift for lambda aliases
* integrated within SAM (Serverless Application Model) framework
* CodeDeploy can grow traffic on an alias gradually over time shifting from another one
	* Linear:
		* Linear10PercentEvery3Minutes
		* Linear10PercentEvery10Minutes
	* Canary (try N percent and switch to 100%):
		* Canary10Percent5Minutes
		* Canary10Percent30Minutes
	* AllAtOnce: immediate
* can create Pre and Post traffic hooks to check health and Rollback on that

## Lambda Limits
* per region
* Execution:
	* memory 128 - 3008MB (64MB increment)
	* Max execution time: 900 seconds (15 minutes)
	* Environment variables (4kb)
	* function container, /tmp direcotry (512MB)
	* Concurrency executions: 1000 (can be increase by request)
* Deployment:
	* Deployment size: 50MB (Compressed), 250 (uncompressed)
	* can use /tmp directory
	* environment variable: 4kb

## Best Practices
* heavy duty and repeatative tasks outside handler
* environment variable for sensitive information (can be encrypted as well)
* Minimize deployment package size to runtime requirements (break down big functions)
* use lambda layers to manage dependencies
* avoid using recursive code!!