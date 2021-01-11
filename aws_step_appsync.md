# AWS Step Functions and AppSync

## Step Functions
* build serverless **visual workflow** to orchestrate Lambda functions
* Represent flow as JSON State Machine
* features:
	* sequencing
	* parallel
	* conditions
	* timeouts
	* error handling
* can integrate with EC2, ECS, on-premise, API Gateway
* maximum execution time 1 year
* can implement human approval
* use cases:
	* order fullfillment
	* data processing
	* web app
	* any workflow
	* additional resources: https://aws.amazon.com/step-functions/use-cases/
* any state can encounter errors:
	* state machine definition issues (eg no matching rule in choice state)
	* task failure (eg exception in lambda)
	* Transient issues (eg network partition events)
* failure in any state will fail the Step Function entirely
* Resolutions to error:
	* Retry: exponential backoff type
	* Catch: move on to next
* best practice is to add error messages
* Types of Step Functions:
	* Standard
		* max duration 1 year
		* execution start rate 2,000/second
		* state transition rate 4,000/account
		* more expensive
	* Express
		* max duration 5 minutes 
		* execution start rate 100,000/second
		* state transition rate nearly unlimited
		* cheaper

## AppSync
* managed service on GraphQL
* it makes easy for applications to get exactly the data they need
* includes combining data from one or more sources
	* NoSQL, RDBMS, HTTP APIs
	* dynamoDB, Aurora, ElasticSearch
	* Custom sources with Lambda
* it can also be used for real-time data retrieval with WebSocket or MQTT on WebSocket
* for mobile apps: local data access and data sync
* start by uploading GraphQL schema
* runs on Resolver (sources)
* Four ways to authorize apps:
	1. API_KEY
	2. AWS_IAM: users/roles/cross-account
	3. OPENID_CONNECT
	4. AMAZON_COGNITO_USER_POOLS
* for custom domain and HTTPS, use CloudFront infront of AppSync