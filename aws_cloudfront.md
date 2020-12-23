# AWS CloudFront

## Intro
* Content Delivery Network (CDN)
* Improves read performance as content is cached at Edge level
* Give DDoS protection, integration with Shield, AWS Web Application Firewall
* can expose external HTTPS and can talk to internal HTTPS backends

## CloudFront Origins
* S3 Bucket
	* for distributing files and cachin them at the edge
	* enhanced security with CloudFront Origin Access Identity (OAI), which allows S3 bucket to communicate only from CloudFront and no where else
	* can use it as ingress to upload files to S3
* Custom Origin (HTTP)
	* ALB
	* EC2 Instance
	* S3 Website (must first enable bucket as static website) 
	* Any HTTP backend

## CloudFront Security
* Geo Restriction
	* can whitelist or blacklist users from a region
	* uses 3rd party Geo-IP database
* HTTPS
	* Viewer Protocl Policy:
		* redirect HTTP to HTTPS
		* use HTTPS only
	* Origin Protocol Policy (HTTP or S3):
		* HTTPS only
		* Match viewer (HTTP to HTTP and HTTPS to HTTPS)
	* S3 Bucket websites does't support HTTPS
 
## CloudFront vs Cross Region Replicaton
* CloudFront:
	* global edge network
	* files are cached for a TTL
	* great for static content
	* contents can be little old
* S3 CRR: 
	* must be setup for each region
	* files are replicated in real-time
	* read only
	* great for dynamic content that needs to be available at low-latency in few regions

## CloudFront Caching
* Cache based on:
	* headers
	* session cookies
	* query string paramters
* it lives at each CloudFront Edge Location
* maximize cache hit and minimize request on origin
* TTL can be set by origin using Cache-Control header, Expires header
* can invalidate part of the cache using CreateInvalidation API
* better to direct static content to S3 Bucket and direct dynamic content (REST and HTTP server) to ALB + EC2 instances

## CloudFront Signed URL/Signed Cookies
* manage who can view the contents
* need to attach a policy:
	* includes URL expiration
	* IP ranges to access the data
	* Trusted signers (who in AWS can create these URLs)
* How long the URL will be valid for:
	* Shared Content: short period of time
	* Private Content: may last for years
* Signed URL: for individual file
* Signed Cookie: for multiple files

## CloudFront Signed URL vs S3 Pre-Signed URL
* CloudFront Signed URL:
	* allow access to path, no matter the origin
	* account wide key-pair, only root can manage
	* can filter by IP, path, date, expiration
	* can leverage caching
* S3 Pre-signed URL
	* issue a request as a person who pre-signed the url
	* uses IAM key to sign
	* limited lifetime

## Notes
* DNS Propagation: https://stackoverflow.com/questions/38735306/aws-cloudfront-redirecting-to-s3-bucket