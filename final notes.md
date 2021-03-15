## A New Post

	• - Max days for EFS IA is 90 days

	• - You can only add one sns or sqs at a time to an S3 event

	• - Rds event subscription is only for sns

	• - Reserved instances cannot be cross region.

	• - Enhanced monitoring is for rds.

	• - VPN conn. Aws, virtual private gateway, VPN conn, customer Gateway, customer network

	• - S3 supports object lock

	• - we cannot have ipv6 cidr only in vpc. Need IPv4 enabled first before ipv6. New EC2 by default uses IPv4

	• - S3 parallel request might give stale data

	• - S3 largest object in a single put is 5gb. But max upload is 5tb which cannot be single put but multi part upload

	• - Max backup for automated one is 35. Using aws backup we can get 90 or more.. And we cannot export automated backup snapshots.. Only manual can be exported

	• - Trarget groups are associated with ELB. And not asg

	• - Active-Active Failover. Use this failover configuration when you want all of your resources to be available the majority of the time. When a resource becomes unavailable, Route 53 can detect that it's unhealthy and stop including it when responding to queries

	• - Amazon Kinesis  is the streaming data platform of AWS and has four distinct services under it: Kinesis Data Firehose, Kinesis Data Streams, Kinesis Video Streams, and Amazon Kinesis Data Analytics

	• - The standby instance will not perform any read and write operations while the primary instance is running

	• - By default, an S3 object is owned by the account that uploaded the object. That's why granting the destination account the permissions to perform the cross-account copy makes sure that the destination owns the copied objects. You can also change the ownership of an object by changing its access control list (ACL) to bucket-owner-full-control. However, object ACLs can be difficult to manage for multiple objects, so it's a best practice to grant programmatic cross-account permissions to the destination account

	• - cfn-init helper script is not suitable to be used to signal another resource. You have to use cfn-signal instead. And although you can use the DependsOn attribute to ensure the creation of a specific resource follows another, it is still better to use the CreationPolicy attribute instead as it ensures that the applications are properly running before the stack creation proceeds

	• - a cluster endpoint (also known as a writer endpoint) simply connects to the current primary DB instance for that DB cluster. This endpoint can perform write operations in the database such as DDL statements, which is perfect for handling production traffic but not suitable for handling queries for reporting

	• - the visibility timeout isn't a guarantee against receiving a message twice

	• - Objects must be stored at least 30 days in the current storage class before you can transition them to STANDARD_IA or ONEZONE_IA. For example, you cannot create a lifecycle rule to transition objects to the STANDARD_IA storage class one day after you create them. Amazon S3 doesn't transition objects within the first 30 days because newer objects are often accessed more frequently or deleted sooner than is suitable for STANDARD_IA or ONEZONE_IA storage.

	• - If you are transitioning noncurrent objects (in versioned buckets), you can transition only objects that are at least 30 days noncurrent to STANDARD_IA or ONEZONE_IA storage

	• - You can use expedited retrievals in Glacier which will allow you to quickly access your data (within 1–5 minutes) 

	• - When you create a step scaling policy, you can also specify the number of seconds that it takes for a newly launched instance to warm up.

	• - The AWS Storage Gateway Hardware Appliance is a physical hardware appliance with the Storage Gateway software preinstalled on a validated server configuration. Can also use Use AWS Storage Gateway with a gateway VM appliance for your compute resources

	• - You do not need to add a random prefix anymore for this purpose since S3 has increased performance to support at least 3,500 requests per second to add data and 5,500 requests per second to retrieve data,

	• - The OS-bypass capabilities of EFAs are not supported on Windows instances. If you attach an EFA to a Windows instance, the instance functions as an Elastic Network Adapter, without the added EFA capabilities

	• - You can't set Lambda@Edge functions as part of your origin group in CloudFront. you can't directly use a single Auto Scaling group as an origin.

	• - data transferred between Amazon EC2, Amazon RDS, Amazon Redshift, Amazon ElastiCache instances, and Elastic Network Interfaces in the same Availability Zone is free. 

	• - efs doesn't natively work with Amazon S3.Amazon FSx for Windows File Server is incorrect because although this service is a type of Amazon FSx, it does not work natively with Amazon S3

	• - You can create a bucket policy to require external users to grant bucket-owner-full-control when uploading objects so the bucket owner can have full access to the objects

	• - load balancers distribute traffic only within their respective regions and not to other AWS regions by default. Although Network Load Balancers support connections from clients to IP-based targets in peered VPCs across different AWS Regions. 

	• - Glacier deep archive has a minimum storage duration of at least 180 days which is only suitable for backup and data archival. If you store your data in Glacier Deep Archive for only 12 hours, you will still be charged for the full 180 days
	
	• CloudFront geo restriction to block the request.This is the easiest and most effective way to implement a geographic restriction for the delivery of content.compared to network acl ip blocking

	• The on-premises servers mount the existing storage using block protocols (iSCSI) volume and file protocols (NFS).

	• AWS DataSync can copy data between Network File System (NFS) shares, Server Message Block (SMB) shares, self-managed object storage, AWS Snowcone, Amazon Simple Storage Service (Amazon S3) buckets, Amazon Elastic File System (Amazon EFS) file systems, and Amazon FSx for Windows File Server file systems


	• With Amazon CloudFront you can set the price class to determine where in the world the content will be cached. Cheaper compared to using all edge locations

	• To make the application instances accessible on the internet the Solutions Architect needs to place them behind an internet-facing Elastic Load Balancer. The way you add instances in private subnets to a public facing ELB is to add public subnets in the same AZs as the private subnets to the ELB. You can then add the instances and to the ELB and they will become targets for load balancing.

	• You cannot create an RDS Read Replica of a database that is running on Amazon EC2. You can only create read replicas of databases running on Amazon RDS.
	
	• You cannot use CloudFront and an OAI when your S3 bucket is configured as a website endpoint
	
	• A partition key is used to group data by shard within a stream. Kinesis Data Streams segregates the data records belonging to a stream into multiple shards. It uses the partition key that is associated with each data record to determine which shard a given data record belongs to.
	• Modify the network ACL on the CloudFront distribution to add a deny rule for the malicious IP address" is incorrect as CloudFront does not sit within a subnet so network ACLs do not apply to it.so for CF need to use WAF
	• The most resilient solution is to configure DX connections at multiple DX locations. This ensures that any issues impacting a single DX location do not affect availability of the network connectivity to AWS
	• You can passthrough encrypted traffic with an NLB and terminate the SSL on the EC2 instances, so this is a valid answer.You can use a HTTPS listener with an ALB and install certificates on both the ALB and EC2 instances. This does not use passthrough, instead it will terminate the first SSL connection on the ALB and then re-encrypt the traffic and connect to the EC2 instances.You cannot use passthrough mode with an ALB and terminate SSL on the EC2 instances
	• You can create a read replica as a Multi-AZ DB instance. Amazon RDS creates a standby of your replica in another Availability Zone for failover support for the replica. Creating your read replica as a Multi-AZ DB instance is independent of whether the source database is a Multi-AZ DB instance
	• FSx works natively with S3 which is also more economical compared to efs 
	• However, to restrict access so that consumers cannot connect to other instances in the VPC the best solution is to use PrivateLink to create an endpoint for the application. The endpoint type will be an interface endpoint and it uses an NLB in the shared services VPC.VPC peering could be used along with security groups to restrict access to the application and other instances in the VPC. However, this would be administratively difficult as you would need to ensure that you maintain the security groups as resources and addresses change
	• There’s no such thing as a DynamoDB Read Replica (Read Replicas are an RDS concept).
	• you cannot use AWS WAF with a network load balancer or classic load balancer
	• you cannot use client-side encryption with keys managed by Amazon S3.
	• server access logging does not have an option for choosing data or management events or log file validation
	• Prefer sns over sqs if there are multiple recipients. Else sqs
	• You cannot increase the number of read transactions per shard. Read throttling is enabled by default for Kinesis data streams. If you’re still experiencing performance issues you must increase the number of shards.
	• An Aurora Replica is both a standby in a Multi-AZ configuration and a target for read traffic.
	• A VPG is used to setup an AWS VPN which you can use in combination with Direct Connect to encrypt all data that traverses the Direct Connect link. This combination provides an IPsec-encrypted private connection that also reduces network costs, increases bandwidth throughput, and provides a more consistent network experience than internet-based VPN connections.
	• Terminate the instance with the least active network connections is not a valid asg scale in
	• you can bring your own IP addresses to AWS Global Accelerator
	• Create a cross-region Aurora Read Replica" is incorrect. This solution would not provide the fast storage replication and fast failover capabilities of the Aurora Global Database
	The steps performed by the custom identity broker to sign users into the AWS management console are:
	- Verify that the user is authenticated by your local identity system
	- Call the AWS Security Token Service (AWS STS) AssumeRole or GetFederationToken API operations to obtain temporary security credentials for the user
	- Call the AWS federation endpoint and supply the temporary security credentials to request a sign-in token
	- Construct a URL for the console that includes the token
	- Give the URL to the user or invoke the URL on the user’s behalf
	• AWS recommend that you use the AWS SDKs to make programmatic API calls to IAM. However, you can also use the IAM Query API to make direct calls to the IAM web service. An access key ID and secret access key must be used for authentication when using the Query API.
	• Solutions Architect should use the SCT to extract and load to Snowball Edge and then AWS DMS in the AWS Cloud. So first sct and the
	• With dedicated hosts billing is on a per-host basis (not per instance).An Amazon EC2 Dedicated Host is a physical server with EC2 instance capacity fully dedicated to your use
	• You cannot point an Alias record directly at an on-premises web server directly. Alias records are used to map resource record sets in your hosted zone to Amazon Elastic Load Balancing load balancers, API Gateway custom regional APIs and edge-optimized APIs, CloudFront Distributions, AWS Elastic Beanstalk environments, Amazon S3 buckets that are configured as website endpoints, Amazon VPC interface endpoints, and to other records in the same Hosted Zone
	• A virtual private gateway is a logical, fully redundant distributed edge routing function that sits at the edge of your VPC. You must create a VPG in your VPC before you can establish an AWS Managed site-to-site VPN connection. The other end of the connection is the customer gateway which must be established on the customer side of the connection
	• EBS optimized instances provide dedicated capacity for Amazon EBS I/O. EBS optimized instances are designed for use with all EBS volume types. And may increase db perf
	With ALB and NLB IP addresses can be used to register:
	- Instances in a peered VPC.
	- AWS resources that are addressable by IP address and port.
	- On-premises resources linked to AWS through Direct Connect or a VPN connection.
	
	• You can control access to files and directories with POSIX-compliant user and group-level permissions. POSIX permissions allows you to restrict access from hosts by user and group.in efs
