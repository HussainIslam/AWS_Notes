# CloudFormation

## Intro
* Infrastructure as code: the code that we write will create and update infrastructure
* this is a declarative way of outlining our AWS Infrastructure
* We can manage almost any resources with it
* Benefits:
	1. no manual creation
	2. Code can be version controlled
	3. changes in infrastructure can be reviewed as cod
* cost can be estimated using CloudFormation template
* productivity:
	* create and delete infrastructure automatically
	* can save a lot of money
	* uses declarative programming; so no need to figure out ordering
* separation of concern: many stacks with many apps and many layers
* there are already many templates online
* upload the template to S3 and reference in CloudFormation
* to update, need to upload new template
* stacks are identified by name
* deleting stacks deletes everything underneath
* Ways of deployment:
	1. Manual:
		* edit in cloudformation
		* use console to input parameters
	2. automated (recommended):
		* edit templates in YAML file
		* use AWS CLI to deploy templates

## building block
* templates components
	* resources: 
		* mandatory section. 
		* aws resources
		* can refer each other
		* over 224 types of resources; not all are supported yet
		* `AWS::aws-product-name::data-type-name`
		* Reference: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html
		* sample: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html
	* parameters: dynamic inputs for templates
		* way to provide inputs
		* useful if template is reused
		* some inputs can't be known ahead of time
		* if a resource config is likely to change, make it a parameter
		* parameters:
			* Types:
				* String
				* Number
				* CommaDelimitedList
				* List<Type>
				* AWS Parameter
			* Descriptions
			* Contraints
			* ConstraintDescription (string)
			* Min/MaxLength
			* Defaults
			* AllowedValues(array)
			* AllowedPattern(regex)
			* NoEcho(Boolean)
		* Pseudo Parameters (AWS provided)
			* AWS::AccountId
			* AWS::NotificationARNs
			* AWS::NoValue (doesn't return a value)
			* AWS::Region
			* AWS::StakId
			* AWS::StackName
	* mappings: static variables for template
		* hard coded variables
		* handy to differentiate between environments (prod/dev), regions, AMI types
		* Example:
		```
		Mappings:
			Mapping01:
				Key01:
					Name: Value01
				Key02:
					Name: Value02
		```
		* great for values known in advance
		* safer control over the template
		* access the map:`Fn:FindInMap` or `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]`
		* Example: `!FindInMap [ Mapping01, Key01, Name ]`
	* outputs: reference to what has been created
		* optional
		* if we output something that can be used in other templates
		* can be viewed in AWS Console or CLI
		* For example: VPC/Subnet IDs
		* A stack that has a reference somewhere else can't be deleted
		* exported output name must be unique within the region
		* Export:
		```
		Outputs:
			StackSSHSecurityGroup:
				Description: Something
				Value: !Ref SomethingElse
				Export:
					Name: SSHGroup
		```
		* Import: using `Fn:ImportValue` or `!ImportValue`
		```
		!ImportValue SSHGroup
		```
	* conditionals: list of conditions to perform resource creation
		* control creation or resource or output based on condition
		* commonly conditions are:
			* environment (dev/prod)
			* AWS Region
			* Any Parameter
		* one condition can reference other conditions, parameter, mapping
		* intrinsic functions can be:
			* Fn::And
			* Fn::Equals
			* Fn::If
			* Fn::Not
			* Fn::Or
	* metadata 
	* Reference of Intrinsic Functions: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html
* templates helpers
	* references
	* functions

## CloudFormation Rollbacks
* Stack Creation Fails:
	* Default: everything rolls back (gets deleted). Check log
	* Option to disable rollback
* Update Fails:
	* Default: rolls back to previous known working state

## Other Concepts
* ChangeSets
	* don't tell whether a update will be successful or not
	* Originl Stack > create change set and view > execute change set to create new update
* Nested Stacks
	* part of other stacks
	* isolate repeated parts
	* LB config that is reused
	* Considered best practice
	* must update the parent to update the nested stack
* Cross Stack vs Nested Stack
	* Cross Stack
		* helpful if stacks have different lifecycles
		* Use Outputs Export and Fn:ImportValue
	* Nested Stack
		* one component is reused
		* importnat only to higher level stack (not shared)
* StackSets
	* Create/Update/Delete stacks across multiple accounts and regions with single operation
	* Need Administrator to create it
	* Trusted accounts can Create/Update/Delete stack instances inside StackSet
	* If a stack is updated, all associated stack instances are updated