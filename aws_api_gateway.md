# API Gateway

## Intro
* can provide REST API Gateway to Lambda function fo the client
* Lambda + Gateway can build a complete serverless application
* has support for WebSocket Protocol
* handle API versioning
* handle multiple environments
* handle security
* create API key, handle request throttling
* Swagger/Open API import to quickly define APIs
* Transform and validate requests and responses
* Generate SDK and API specifications
* Cache API responses
* Integrate with:
	* Lambda Function
	* HTTP (any HTTP endpoints)
	* AWS Services API
* Types:
	* Edge-optimized (default)
		* for global clients
		* requests are routed through CloudFront Edge locations
		* the Gateway still lives in one region
	* Regional
		* client in same region
		* can manually combine with CloudFront (more control over caching strategies and distribution)
	* Private
		* can only be accessed from within VPC using ENI
		* use resource policy to define access
* Deployment
	* need to deploy API to make them effective
	* changes are deployed to stages
	* each stage has their own configuration parameters
	* stages can be rolled back because history of deployments is kept
* Stage Variables
	* these are like environment variables
	* passed to the "context" object of Lambda function
	* use them to change often changing configuration values
	* can be used in:
		* Lambda Function ARN
		* HTTP Endpoint
		* Parameter mapping templates
	* Use cases:
		* configure HTTP endpoints the stage talk to (dev/prod)
		* pass config parameters to AWS Lambda through mapping templates
		* use this to indicate the corresponding Lambda alias which will point to a Lambda Function

## Canary Deployment
* possibility to enable canary deployment for any stage
* choose % of traffic the canary channel receives
* metrics and log will be separate
* possibility of override stage variables for canary
* blue/green deployment with Lambda and Gateway

## Integration
* Integration Type MOCK
	* mock integration
	* Gateway returns a response without sending the request to backend
* Integration Type HTTP/AWS
	* must configure both integration request and response
	* setup data mapping using mapping templates for request and response
* AWS_PROXY (Lambda Proxy)
	* reqeusts from client is input to lambda
	* function is responsible for logic of request/response
	* no mapping template, headers, query string parameters; these are passes as arguments
* HTTP_PROXY
	* no mapping template
	* HTTP request is passed to the backend
	* the response is forwarded by API Gateway
* Mapping Templates
	* for AWS and HTTP integration
	* can be used to modify request/response
	* rename/modify query string parameters
	* modify body content
	* add headers
	* uses Velocity Template Language (VTL), which is scripting language
	* can filter out results
	* Use case: convert SOAP to REST; map query string parameter to json document

## API Swagger/Open API spec
* common way fo defining REST APIs, using API definition as code
* Import existing Swagger/Open API spec to API gateway, includes:
	* method
	* method request
	* integration request
	* method response
	* AWS extensions of API Gateway
* can export current API as Swagger/Open API spec
* can be written in YAML or JSON
* can generate code for app using Swagger

## Caching API Responses
* API gateway will get responses from the cache
* Default TTL is 300 seconds; max 3600s
* Caches are defined at stage level
* can be override at method level
* can encrypted
* size between 0.5GB to 237GB
* cache is expensive
* makes sense in Prod
* can flush the entire cache (invalidate) immediately
* clients can invalidate cache with `header: Cache-Control:max-age=0` (need IAM auth)
* not imposing InvalidateCache policy can enable any client to invalidate the cache

## Usage Plans and API Keys
* can make API available as an offering to customers
* Usage plan:
	* who can access one or more deployed API stages and methods
	* how much and how fast they can access
	* use API keys to identify clients and meter access
	* configure throttling limits and quota limits that are enforced on individual client
* API Keys:
	* strings distributed to customers to authenticate users
	* can be used with usage plan
	* throttling limits are applied to API keys
	* Quotas limits is overall number of maximum requests
* configure usage plan
	1. create one or more APIs, config methods to require keys, and deploy APIs
	2. generate or import api keys to distribute to develpers (customers)
	3. create usage plant with desired throttle and quota limits
	4. associate API stages and keys with usage plan
* must provide the key in `x-api-key` header of request

## Monitoring and Logging
* CloudWatch Logs
	* enable logging at stage level
	* can override at method level
* X-Ray
	* enable to get extra information
	* X-Ray on Gateway + Lambda give full picture
* CloudWatch metics
	* metrics are by stage
	* possible to enable detailed metrics
	* major metrics: 
		* `CacheHitCount` 
		* `CacheMissCount`
		* `IntegrationLatency`: time between relay request and receives response from backend
		* `Latency`: time between receiving a request and sendin resopnse from client; more than `IntegrationLatency`
	* errors: 
		* 4xxErrors (client side)
			* 400: bad request
			* 403: access denied
			* 429: too many reqeust
		* 5xxErrors (server side)
			* 502: bad gateway exception
			* 503: service unavailable
			* 504: integration failure (endpoint reqeust timed out)
* throttling
	* throttles requests at 10000 reqeusts per second accross all API
	* can be requested to increase
	* error response: 429 (too many reqeusts)
	* improve performance by Stage limit and method limits or usage plans
	* one overloaded API can hamper others

## CORS
* can be enabled API calls from another domain
* options pre-flight request must contain following headers:
	* `Access-Control-Allow-Methods`
	* `Access-Control-Allow-Headers`
	* `Access-Control-Allow-Origin`

## Security
* IAM Permissions
	* need to be attached to user/role
	* authentication: IAM
	* authorization: IAM Policy
	* use Sig v4 to use the IAM in request header
* Resource policies
	* who and what can access resources
	* allow for cross account access with IAM Security
	* allow for specific IP
	* allow for VPC endpoint
* Cognito User Pools
	* database of users
	* manage user lifecycle
	* API Gateway verifies identity automatically from Cognito
	* can use social media login
	* no custom integration needed
	* Authentication: Cognito user pools
	* Authorization: API Gateway methods
* Lambda Authorizer (Custom Authorizer)
	* Token based authorizer (bearer token) (JWT or Oauth)
	* request parameter based Lambda Authorizer (headers, query string, stage var)
	* Lambda must return an IAM policy for user; result policy is cached
	* authentication: external
	* authorizatin: lambda function (need to code)

## HTTP API vs REST API vs WebSocket
* HTTP API
	* low latency, cost effective AWS Lambda Proxy, HTTP Proxy and private integration
	* Supports OIDS and OAuth
	* built-in support for CORS
	* no usage plans
	* very cost
* REST API
	* all features
	* doesn't support Native OpenID Connect/OAuth
* WebSocket:
	* two way interactive communication between browser and server
	* server can push info to client
	* enables 'stateful' application use cases
	* for real-time applications
	* works with any kind of integrations (AWS services or HTTP endpoints)