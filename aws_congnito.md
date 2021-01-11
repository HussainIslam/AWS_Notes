# Cognito

## Intro
* used to provide application users an identity to interact with the app
* Cognito User Pools
	* sign in functionality
	* integrate with API Gateway and ALB
* Identity Pool
	* used to give users credentials to access resources
	* integrate with Cognito User Pools to use as an identity provider
* Cognito Sync
	* sync data from device to Cognito
	* is deprecated
	* replaced by AppSync

## Cognito User Pools (CUP)
* create a serverless database of users for the app
* Simple Login: username(email) or password
* Password reset
* email and phone verification
* Multi-Factory auth (MFA)
* Federated identities: Facebook, Google, SAML
* can block users if credentials compromised
* Login action sends back JWT(JSON Web Token)
* have internal database of users
* integrates with Gateway and ALB
* Lambda Triggers: Lambda function can be invoked synchronously on these triggers
	* Authentication Events:
		* Pre Authentication
		* Post Authentication
		* Pre Token Generation
	* Sign up
		* Pre sign up
		* Post confirmation
		* Migrate user
	* Messages
		* Custom messages
	* Token creation
		* Pre Token generation
* Hosted Authenticaiton UI
	* it has hosted authentication UI for signup and signin

## Cognito Identity Pools (federated identities)
* identities for users on temporary basis
* identity pool can include:
	* public providers (amazon, facebook, google, apple)
	* users in Cognito User pool
	* OpenID Connect Providers and SAML identity providers
	* developer authenticated identities (custom login server)
	* also allow for guest access
* users can access AWS services directly or through API Gateway
	* IAM policies applied are defined in Cognito
	* policies can be customized at user level
* can define Default IAM roles and also assign based on user_id
* user access can be partitioned based on policy variables
* IAM credentials are obtained by Cognito Identity Pools through STS
* Roles must have "trust" policy of Cognito Identity Pools

## User Pool vs Identity Pool
* User Pool
	* database of users for app
	* allows federated logins
	* customize UI login
	* has triggers with Lambda
	* manager username/password
* Identity Pools
	* Obtain credentials for users
	* users can login through public social, OIDC, SAML, and User Pools
	* can have guest users
	* mapped to IAM roles and policies
	* gives access to AWS services

## Cognito Sync (Deprecated)
* store preferences, config, state of app
* Cross device sync
* Offline capability 
* store data in datasets up to 1MB, up to 20 datasets to sync
* Push Sync: silently notify all devices when identity data changes
* Cognito Stream: stream data from Cognito into Kinesis
* Cognito Events: execute Lambda in response events