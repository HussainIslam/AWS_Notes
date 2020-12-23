# Amazon S3

## Buckets
* allows people store object(files) in buckets (directories)
* globally unique name
* defined at regional level
* Naming conventions:
	* no uppercase
	* no underscore
	* 3-63 characters
	* not an ip
	* start with lower case letter or number
* objects have key. the key is FULL path of that object inside the bucket. The key can have Prefix (path inside the bucket) and Object name
* there's no concept of directories inside bucket
* object values are content of the body:
	* max object size 5TB
	* cant upload more than 5GB. Must upload using "multi part upload"
* Objects can have metadata (key-value pairs upto 10)
* Objecsts can also have tags and version ID
* The bucket is private by default. So the objects in it will not be accessible by public.

## Versioning
* files in S3 can be versioned
* enabled at bucket level
* if some file exists, it will not overwrite rather create versions
* it is a best practice
	* protect against unintended deletion
	* easy roll back to previous versions
* files that are not versioned before enabling it will have "null" as version
* suspending doesn't delete versions

## Encryption
* There are 4 methods of encryption
	1. SSE-S3: uses keys handled and managed by AWS
		* sever side encryption
		* AES-256 encryption
		* must set header: "x-amz-server-side-encryption":"AES256"
		* because of the header, when an object is uploaded to S3, it will know how to encrypt it and process accordingly
	2. SSE-KMS: uses AWS Key Management Service
		* gives control on user
		* give audit trail
		* server side encrytion
		* must set header: "x-amz-server-side-encryption":"aws:kms"
		* the header instructs AWS to encryption
	3. SSE-C: client manages their own keys
		* AWS S3 doesn't store the ecryption keys
		* HTTPS must be used
		* encyrption key must be provided in request header
		* to retrieve the object, the key must also be provided
	4. Client Side Encryption
		* encrypt the object before uploading
		* clients must decrypt the data themselves
		* Amazon S3 Encryption Client can provide the help
* Encryption in transit:
	* Amazon S3 exposes
		* HTTP endpoint: non encrypted
		* HTTPS endpoint: encryption in flight
	* HTTPS recommended and default, which is mandatory for SSE-C
	* also called SSL/TLS

## S3 Security
* User based
	* IAM policy: which API calls should be allowed for a specific user
* Resource based:
	* Bucket Policy: bucket wide rules from S3 console, allows cross account access
		* https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html
		* https://aws.amazon.com/blogs/security/how-to-prevent-uploads-of-unencrypted-objects-to-amazon-s3/
		* Bucket Policy Generator: https://awspolicygen.s3.amazonaws.com/policygen.html
		* JSON based
		* Resources: buckets and objects
		* Actions: set of API to Allow/Deny
		* Effect: Allow/Deny
		* Principal: account or user to appy the policy to
		* used to grant public access
		* force objects to be encrypted at upload
		* grant access to another account
	* Object ACL: finer than bucket policy
	* Bucket ACL: less common
	* Bucket Settings for block Public Access (four types):
		* new access control lists (ACLs)
		* any access control lists (ACLs)
		* new public bucket or access point policies
		* any public bucket or access point policies
		* there four are used so the bucket will never be public and can be set at account level
* An user can access an object if IAM permissions allow or the resource policy allows; and there's no explicit DENY
* Networking: Support VPC Endpoints
* Loggin and Audit: 
	* S3 Access Logs can be stored in Bucket
	* API calls can be logged in CloudTrail
* User Security:
	* MFA Delete: MFA can be required in versioned buckets to delete objects
	* Presigned URL: valid for a limited time. use case: premium video service for logged in users

## S3 Websites
* S3 can host static websites and have them accessed from internet
* the website URL will be:
	* <bucket-name>.s3-website-<aws-region>.amazonaws.com, or
	* <bucket-name>.s3-website.<aws-region>.amazonaws.com
* if website is enabled but the bucket policy is not allowed for public reads, then the user will get a 403 (Forbidden) error.

## Cross Origin Resource Sharing (CORS)
* origin is a scheme (protocol), host(domain), and port
* this means we want to get resources from a different origin
* this is a browser based security to allow requests to other origins while visiting the main origin
* The request will not be fulfilled unless the other origin allows 
* CORS Headers: Access-Control-Allow-Origin
* in case of CORS, the web browser makes a "Preflight Request" to the other origin and the other origin responds with the original hostname and methods that can be used (GET, PUT, DELETE, for example). After this, web browser can do the actual request.

## S3 CORS
* to do cross-origin requests, we need to enable correct CORS headers
* we can allow for specific origin or all origin (`*`)

## S3 - Consistency Model
* read after write consistency for PUTS of new object (Eventually Consistent)
	* we can retrieve an object as soon as we write it: PUT 200 lead to GET 200
	* if we do a GET before PUT we will get a 404. If we do another GET after the PUT, even though the object exists, we would get a 404. 
* Eventually consistency for DELETES and PUTS of existing objects
	* if we read an object after updating, we might get the olde version. PUT 200 => PUT 200 => GET 200. May need to wait for some time
	* if we delete an object, we might still be able to retrieve it for a short time: DELETE 200 => GET 200
* There's not way to get "Strong Consistency"

## S3 MFA - Delete
* need to enable Versioning on the S3 bucket
* Need MFA to 
	* permanently delete an object version
	* suspend versioning
* Don't need MFA to
	* enable versioning
	* listing deleted verion
* Only Root can enable/delete MFA-Delete
* this can be enabled only through CLI

## S3 Access Logs
* log all access to S3 Buckets
* any request made to S3, from any account, authorized or denied will be logged into another S3 Bucket
* The data can be analyzed using data analysis tools or Amazon Athena
* Log Format: https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html
* Don't set the logging bucket to the bucket being monitored
* This option can be found in Properties of the bucket called "Server Access Logging"

## S3 Replication (CRR & SRR)
* Asynchronous replication to another bucket in same region or not
* CRR = Cross Region Replication
* SRR = Same Region Replication
* Must enable versioning in source and destination
* Bucekts can be in different accounts
* must give proper IAM permission to S3
* CRR use case: compliance, lower latency access, replicaiton across accounts
* SRR use case: log aggregation, live replication between prod and test
* Only new objects will be replicated after enabling it
* delete operations are not replicated (we can enable marker replicaiton but theres no way to replicate permanent delete)
* no chaining of replication
* https://docs.aws.amazon.com/AmazonS3/latest/dev/replication.html

## S3 Pre-signed URLs
* can be generated using SDK/CLI
	* download: better to use CLI
	* upload: must use SDK
* valid for 3600 seconds, can be changed with `--expires-in <time>` argument
* users given the url inherit the permissions of the creator
* Use case:
	* only allow logged in users to download premium content
	* allow an ever changing list of users to download a file
	* allow temporarily a user to upload a file to location
* `aws s3 presign s3://mybucket/myobject --region my-region --expires-in 300`

## S3 Storage Classes
* Amazon S3 Standard - General Purpose
	* high durability (11 9's) of objecs multiple AZ
	* sustain 2 concurrent facility failures
	* big data, mobile/gaming apps, content distribution
* Amazon S3 Standard - Infrequent Access (IA)
	* less frequent but rapid access
	* high durability
	* lower cost compared to General purpose
	* sustain 2 concurrent failures
	* disaster recovery, backups
* Amazon S3 One Zone - Infrequent Access
	* stored in signle AZ; data lost if AZ is destroyed
	* low latency and high throughput
	* supports SSL for data transit and encryption
	* less availability (99.5%)
	* secondary backup, data that can be recreated
* Amazon S3 Intelligent Tiering
	* high throughput and low latency
	* small monthly monitoring fee and auto-tiering fee
	* automatically moves objects between two access tiers bese on changing access patterns
	* high durability
	* resilient against events affecting AZ
	* 99.9% availability over a given year
* Amazon Glacier
	* cold archieve
	* low cost object storage for backup/archiving
	* data is retained for longer term
	* alternative to on-premise magnetic tape
	* very high durability
	* low cost per storage per month + retrival cost
	* each item in Glacier is called "Archive" (upto 40TB)
	* Archives are stored in Valuts
	* Three retrieval options:
		1. Expedited (1-5 minutes)
		2. Standard (3-5 hours)
		3. Bulk (5 - 12 hours)
	* Minimum storage duration 90 days
* Amazon Gracier Deep Archive
	*  for longer term storage
	* cheaper
	* two retrieval options:
		1. Standard (12 hours)
		2. Bulk (48 hours)
	* minimum storage duration 180 days
* Amazon S3 Reduced Redundancy Storage (Deprecaed)
![Storage Classes Comparison](https://jayendrapatil.com/wp-content/uploads/2016/03/S3-Storage-Classes-Performance.png)

## Moving Between Storage classes
* objects can be moved between storage classes
* moving objects can be automated using lifecycle configuration
* Transition Actions can be defined, which is when to move an object from one class to another
* Expiration Actions can be used to delete log files or old versions of a file. It can also be used to clean incomplete multi-part uploads
* Rules can be applied to specific prefix (.mp3) and file tag (finance)

## S3 Performance
* Baseline performance
	* auto scales up to high request rates (latency 100-200ms)
	* applications can achieve at least 3500 PUT/COPY/POST/DELETE and 5500 GET/HEAD requests per second per prefix in a bucket
	* in `bucket/folder1/sub1/file`, `bucket/folder1/sub1/` is the prefix
* KMS Limitation
	* performance may be affected KMS limits, if SSE-KMS is used
	* when uploading, GenerateDataKey KMS API is called
	* When downloading, Decrypt KMS API
	* KMS quota is based on region, ranging from 5500 to 30000 
	* quota request can't be increased
* Multi-part upload
	* recommended for files over 100MB; must for files over 5GB
	* Can help parallelize and incease the transfer speed
* S3 Transfer Acceleraiton (upload only)
	* increase transfer speed by first transferring file to a Edge location and forwarding that to S3 Bucket
	* compatibe with multi-part upload
* S3 Byte-Range Fetches
	* parallelize GETs by specifying byte ranges
	* beter resilience in case of failures
	* can speed up downloads
	* only retrieve only partial data (head of the file)

## S3 Select and Glacier Select
* retrieve less data using SQL by performing server side filtering
* can filter by rows and columns
* less network traffic, less CPU cost at client-side

## S3 Event Notifications
* Notification and actions can be based on:
	* actions/notification: creation, deletion, restoration,
	* type of object: .jpg file
	* generate thumbnails 
	* Targets:
		1. SNS (Simple Notification Service): send notification to emails
		2. SQS (Simple Queue Service): send notifiaction to a queue
		3. Lambda Function: generate some custom code
	* can create as many notifications as we want
	* if two writes on single non-versioned object is done at the same time, one notification COULD be generated

## AWS Athena
* serverless service to perform analytics agains S3 files
* SQL to query files
* JDBC/ODBC drivers to connect
* charged per query and amount of data scanned
* support CSV, JSON, ORC, Avro, Parquet (built on Presto)
* Use cases: Business intelligence/analytics/reporting, VPC flow logs, ELB logs, CloudTrail
* Additional Resources: https://aws.amazon.com/premiumsupport/knowledge-center/analyze-logs-athena/

## S3 Object Lock and Glacier Vault Lock
* S3 Object Lock
	* adopt a WORM (Write Once, Read Many) model
	* block object version deletion for a specified time
* Glacier Vault Lock
	* same WORM model
	* lock the policy for future edits and can't be changed
	* helpful for compliance and data retention