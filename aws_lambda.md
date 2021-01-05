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