# Serverless Application Model

## Intro
* framework for developing and deploying serverless applications
* all configuration is in YAML code
* this will generate CloudFormation code from the YAML file
* Supports anything from CloudFormation: Outputs, Mappings, Parameters, Resources
* only two commands to deploy to AWS
* can use use CodeDeploy to deploy Lambda functions
* can help us run Lambda, Gateway, DynamoDB locally
* Recipe:
	* 'Transform' header indicates that it is SAM template:
		* `Transform: 'AWS::Serverless-2020-10-31'`
	* Helpers for resources
		* AWS::Serverless::Function
		* AWS::Serverless::Api
		* AWS::Serverless::SimpleTable
	* Package and Deploy:
		* CloudFormation or SAM Package
			* This will zip SAM Template, Application Code, and Swagger File and upload to S3
			* Convert SAM Template to CloudFormation Template
			* generated template will refer to S3 bucket
		* CloudFormation or SAM Deploy
			* this will generate and execute a 'change set'
			* apply the change set to the stack

## SAM Policy Templates
* list of templates to apply permissions to lambda functions
* defines what lambda functions can do
* additional resources: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-policy-templates.html
* important examples:
	* S3ReadPolicy: read only permissions to objects in S3
	* SQSPollerPolicy: allows to poll SQS Queue
	* DynamoDBCrudPolicy

## SAM and CodeDeploy
* SAM natively uses CodeDeploy to update Lambda
* define pre and post traffic hooks features to validate deployment
* automated rollback using CloudWatch Alarms