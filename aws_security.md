# Security

## Encryption
* Encryption in flight (SSL)
	* encrypt data before sending
	* use SSL certificates
	* ensures protection from Man-in-the-middle attack
* Server-side encryption at rest
	* data encrypted at server
	* decrypted before sending
	* encryption and decryption with a key
	* the key is managed by Key Management System (KMS)
* Client-side encryption
	* encrypted by the client
	* never decrypted by the server
	* decrypted by the receiving client
	* can use Envelope Encyption

## Key Management Service (KMS)
* in AWS, encryption is synonymous to KMS
* AWS manages keys for us
* integrated with IAM
* can be used with CLI/SDK
* when exceed request quota get a ThrottlingException + exponential backoff
* cryptographic operations share a single quota, including requests made by AWS
* for GenerateDataKey, better use DEK Caching
* can also request quota increase by API/Ticket
* Customer Master Keys (CMK) types:
	1. Symmetric (AES-256 bit Keys)
		* single key to encrypt and decrypt
		* first offering of KMS
		* many services use this under the hood
		* never get access to unencypted key (must call KMS API)
	2. Asymmetric (RSA and ECC Key pairs)
		* two keys: Public key (encrypt) and Private Key (decrypt)
		* SSL uses this

## Symmetric Keys
* able to manage keys and policies:
	* create
	* rotation policies
	* disable
	* enable
* audit key usage with CloudTrail
* Three types of CMK:
	1. AWS managed service Default CMK (Free)
	2. User created Key: $1/month
	3. User imported (must be 256bit symmetric): $1/month
* pay  $0.03/10000 API calls
* user should never be able to retrieve the key
* encypted secrets should be stored in environment variables
* KMS can encrypt up to 4KB per call
* anything over 4KB, need Envelope Encryption
* Giving access to KMS to someone:
	* Key policy allows
	* IAM policy allows
* KMS Keys are bound to a region

## KMS Key Policies
* control access to KMS keys
* cannot access without Policies
* Default KMS Key policy:
	* created if a policy is not specified
	* complete access to the key to the root user
	* give access to IAM policies to KMS Key
* Custom KMS Key Policy:
	* define users, roles that can access the KMS Key
	* define who can administer
	* useful for cross-account access of KMS Keys

## Envelope Encyption
* to encrypt over 4KB
* API for this: GenerateDataKey
* encryption happens client-side
* can be implemented using Encryption SDK/CLI
* also has Data Key Caching (reuse data key)

## SSM Parameter Store
* secure storage for config and secrets
* serverless, scalable, durable, easy SDK
* optional encryption using KMS
* versioning of config/secrets
* config management using path & IAM
* notification with CloudWatch Events
* integration with CloudFormation
* Tiers:
	* Standard
		* total parameter: 10,000
		* max size of value: 4KB
		* parameter policies not available
		* storage pricing free
		* standard throughput free but $0.05 per 10,000 over 1000 transactions per second
	* Advanced
		* total parameter: 100,000
		* max size of value: 8KB
		* parameter policies available
		* storage not free
		* throughput $0.05 per 10,000
* Parameter policies allow TTL. can assign multiple policies at a time

## AWS Secrets Manager
* stores secrets
* can force rotation after X days
* automate generation of secrets on rotation using Lambda
* integrate with RDS
* encrypted using KMS
* mostly meant for RDS integration

## SSM Parameter Store vs Secrets Manager
* Secrets Manager:
	* expensive
	* auto rotation
	* KMS mandatory
	* integration with RDS
	* integation with CloudFormation
* SSM Parameter Store
	* less expensive
	* no secret rotation
	* KMS optional
	* integration with CloudFormation
	* can pull Secrets Manager secret using Parameter Store API

## CloudWatch Logs Integration
* can encrypt CloudWatch logs with KMS keys
* encryption is enabled at log group level by associating a CMK with a log group, either when creating or after. has to use CloudWatch Logs APIs:
	* associate-kms-key (for existing log group)
	* create-log-group (if log group doesn't exist)

## CodeBuild Security
* to access resources in VPC, specify VPC config for CodeBuild
* don't store them as plaintext in environment variables
* suggested way to store environment variables:
	* refer to parameter store parameters
	* refer to secrets manager secrets