# Miscellaneous Services

## Simple Email Service
* send email to people
* using:
	*  SMTP
	* AWS SDK
* receive email integrated with:
	* S3
	* SNS
	* Lambda
* need IAM permission

## Database Summary
* RDS: relational database; good for OLTP (on-line transaction processing)
* DynamoDB: noSQL, managed, key-value, serverless
* ElastiCache: in-memory, redis/memcached, for caching
* Redshift: OLAP (on-line Analytic Processing); data warehousing/data lake
* Neptune: Graph database
* DMS: Database migration service
* DocumentDB: managed MongoDB for AWS

## AWS Certificate Manager (ACM)
* host public SSL certifcates in AWS
* two ways to manage:
	* buy and upload using CLI
	* Let ACM provision and renew public SSL (for free)
* will load them:
	* Load Balancer including EB created
	* CloudFront
	* APIs on API gateways