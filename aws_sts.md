# Security Token Service

## STS
* Allows to grant limited and temp access to AWS resources (up to 1 hour)
* APIs:
	* AssumeRule: within or cross account (important)
		* define IAM role
		* define which principals can access 
		* use STS to retrieve credentials and impersonate the IAM role
		* valid from 15-60 minutes
	* AssumeRoleWithSAML: return credentials for users logged with SAML
	* AssumeRoleWithWebIdentity: return credentials for users logged with facebook, google
	* GetSessionToken: for MFA(important)
		* need IAM Policy with IAM Conditions
		* in IAM Policy: `aws:MultiFactorAuthPresent:true`
		* returns:
			* AccessID
			* Secret Key
			* Session Token
			* Expiration date
	* GetFederationToken
	* GetCallerIdentity: return details about IAM user/role(important)
	* DecodeAuthorizationMessage: decode error messages when API is denied (important)