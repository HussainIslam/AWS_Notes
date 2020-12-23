# AWS CLI

## Intro
* Developing and performing AWS tasks can be done in:
	* AWS CLI on local computer
	* AWS CLI on EC2 machines
	* AWS SDK on local computer
	* AWS SDK on EC2 machines
	* AWS Instance Metadata Service on EC2

## AWS CLI setup (Local Computer)
* Find the user Security Credentials on IAM under 'Security Credentials' section of 'User'. 
* Create a new "Access Key"
* on Terminal of your computer run `aws configure`
* Paste in the ID of the access key and press enter
* Paste in the Access Key and press enter
* Enter the name of the AZ
* Enter format (default is JSON)

## AWS CLI setup (EC2 Instance)
**Must be done with IAM Roles**
* ssh into the EC2 instance
* `aws configure` the instance WITHOUT any ID and access key
* Go to IAM > Roles > Create Role > AWS Service
* Select EC2 and click Next
* Select a Policy: example: AmazonS3ReadOnlyAccess. Click Next > Next
* Enter Name and Description and click Create Role
* Go to EC2 and right click on the the Instance
* Security > Modify IAM Role
* Select the created IAM Role and Save
* Now in our terminal, if we type `aws s3 ls` we should be able to see results

## AWS CLI Profile
* creating profile: `aws configure --profile <profile-name>`
* run a command with a profile: `aws s3 ls --profile <profile-name>`

## AWS CLI with MFA
* must create a temporary session which can be done by running `STS GetSessionToken` API call
* `aws sts get-session-token --serial-number <arn-of-mfa-device> --token-code <code-from-token> --duration-seconds <number-of-seconds>`
* ARN of MFA device can be found in IAM User (if MFA device is not setup, will need to create a MFA device first)
* This will return token details, which need to be added to the `~/.aws/credentials` file using a text editor.
* Sample:
```
[mfa]
aws_access_key_id = <string>
aws_secret_access_key = <string>
aws_session_toke = <string>
```

## AWS SDK
* used to perform actions on AWS from applications
* Official SDK supports:
	* Java
	* .NET
	* Python (boto3/botocore)
	* Node.js
	* C++
* `us-east-1` is default region

## AWS Limits (Quotas)
* API Rate Limit
	* DescribeInstances API for EC2 has 100 call/second
	* GetObject on S3 has 5500 GET/second per prefix
	* If go over limit (intermittent): implement Exponential Backoff
	* If go over limit (consistent): request throttling limit increase
* Service Quotas/Limits:
	* we can run 1152 vCPU of on-demand Standard instances
	* can request service limit increase by opening a ticket
	* can request service quota increase by using service quotas API
* Exponential Backoff
	* ThrottlingException once in a while
	* This is based on "Retry" mechanism
	* Must implement manually using API as is or specific cases
	* basically doubles the time before requesting

## AWS CLI Creditals Provider Chain
* CLI will look for credentials in the following order:
	1. command line options (region/output/profile)
	2. Environment variables (AWS_ACCESS_KEY_ID and etc)
	3. CLI Credential file (`~/.aws/credentials`)
	4. CLI configuration file (`~/.aws/config`)
	5. Container credentials (for ECS tasks)
	6. Instance profile credentials (for EC2 Instance Profiles)
* AWS SKD order:
	1. Environment Variables
	2. Java System Properties
	3. Default credential profiles file
	4. Amazon ECS Container Credentials (ECS containers)
	5. Instance profile credentials (EC2 instances)

## Create Policies
* Can be created using "Visual Editor" or typing in JSON. We can also use the policy generator.
* We can also create an inline policy that can be used only with one role
* To check policies through AWS CLI we can add option `--dry-run`

## AWS CLI STS Decode Errors
* API calls might give long error messages, which can be decoded using the STS command line.
* We would need to add this permission to the policy to decode the messages
* `aws sts decode-authorization-message --encoded-message <message>`
* referece: https://docs.aws.amazon.com/cli/latest/reference/sts/decode-authorization-message.html

## Signing AWS API Requests
* should sign AWS HTTP requests using Signature v4 or SigV4

## EC2 Instance Metadata
* EC2 instances can learn about themselves
* URL is `http://169.254.169.254/lates/meta-data`
* this is an internal url and will not work from outside. So, we can SSH into and EC2 Instance and run `curl http://169.254.169.254/latest/meta-data/hostname`
* Metadata is info about the EC2 Instance

## Notes
* AWS CLI Reference: https://docs.aws.amazon.com/cli/latest/reference/
* IAM Policy Simulator Guideline: https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html
* IAM Policy Similator: https://policysim.aws.amazon.com/home/index.jsp?#