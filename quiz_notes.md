- AWS lambda does encrypt env variables by default. But it uses a default key for this. So the values are visible to anymore who uses lambda console. If we need more protection, use KMS and create new key and use it to encrypt env variable. Now env variables are encrypted in lambda console as well. Use encryption helpers for this.

- CloudFront signed URLs and signed cookies provide the same basic functionality: they allow you to control who can access your content. Use signed cookies for the following cases:You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of a website.Use signed URLs for the following cases:You want to use an RTMP distribution. Signed cookies aren't supported for RTMP distributions.You want to restrict access to individual files, for example, an installation download for your application. Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.

-  using an IAM Role is a more suitable choice over a resource-based policy for the Amazon ECS task execution role.

- To give access to corp users: The correct answers are: Setup a Federation proxy or an Identity provider,  Setup an AWS Security Token Service to generate temporary tokens,Configure an IAM role and an IAM Policy to access the bucket.

-  maximum days for the EFS lifecycle policy is only 90 days. Amazon EBS costs more than S3 and is not as scalable as Amazon S3. It has some limitations when accessed by multiple EC2 instances. So scheduld backup of ebs is costlier than s3.

- CF origin failover is much more cost effectie than multi region deployment.

- Amazon Macie is an ML-powered security service that helps you prevent data loss by automatically discovering, classifying, and protecting sensitive data stored in Amazon S3. Rekognition is simply a service that can identify the objects, people, text, scenes, and activities, as well as detect any inappropriate content on your images or videos.

- enabling IAM DB Authentication, so that db can only be accessed using iam creds. So now we can set up smae ec2 iam role creds are required by users to access db. Ec2 uses the role and others using console to access db need to use same role. Even if we set up role for ec2 to access db, we need to set  up db auth for console access.

- Amazon FSx For Lustre is a high-performance file system for fast processing of workloads. Lustre is a popular open-source parallel file system which stores data across multiple network file servers to maximize performance and reduce bottlenecks.

- CPU% and MEM% metrics are not readily available in the Amazon RDS console, enabling Enhanced Monitoring in RDS gives these data.

- You can use CloudWatch Events to run Amazon ECS tasks when certain AWS events occur. You can set up a CloudWatch Events rule that runs an Amazon ECS task whenever a file is uploaded to a certain Amazon S3 bucket using the Amazon S3 PUT operation. No need for lambda for this.


- need to use Geoproximity Routing and specify a bias to control the size of the geographic region from which traffic is routed to your instance. You can also optionally choose to route more traffic or less to a given resource by specifying a value, known as a bias.Geolocation Routing is incorrect because you cannot control the coverage size from which traffic is routed to your instance in Geolocation Routing.

-  with AWS DataSync, you can transfer data from on-premises directly to Amazon S3 Glacier Deep Archive. 

- creating a bucket policy for s3 is time consuming if we have lots of bucket. So creating an endpoitn policy is faster in such scenarios

- an MX record specifies the mail server responsible for accepting email messages on behalf of a domain name. Also There is no constraint that the S3 bucket must be in the same region as the hosted zone, in order for the Route 53 service to route traffic into it.

-  moving to S3 Glacier is more expensive than directly backing it up to Glacier Deep Archive. Snowball Edge can't directly integrate backups to S3 Glacier. it is difficult to directly integrate a tape backup solution to S3 without using Storage Gateway

- When failing over, Amazon RDS simply flips the canonical name record (CNAME) for your DB instance to point at the standby, which is in turn promoted to become the new primary.with multi-AZ enabled, you already have a standby database in another AZ and we can switch to that in case of primary failure.

- we can  Set the filter policies in the SNS subscriptions to publish the message to the designated SQS queue based on a criteria

- There is no direct way for CloudWatch Events to monitor the status of your Route 53 endpoints. Route53 is best option for active-passive failover.

- as classic load balacner doesnt support sni, use a cloudfront distribution and associate it with sni

- You can associate the CreationPolicy attribute with a resource to prevent its status from reaching create complete until AWS CloudFormation receives a specified number of success signals or the timeout period is exceeded. 

- s3 creates event Delete for object deletion and DeleteMarker when a versioned object delete is done.

- your network ACL must have an outbound rule to allow ephemeral ports (32768 - 65535). These are the specific ports that will be used as the client's source port for the traffic response.In the Network ACL, update the rule to allow inbound TCP connection on port 443 from source 0.0.0.0/0 and outbound TCP connection on port 32768 - 65535 to destination 0.0.0.0/0

- you still cannot integrate DynamoDB table with CloudFront as these two are incompatible.

- by default, Auto Scaling is not enabled in a DynamoDB table which is created using the AWS CLI.

- you cannot set up an Active-Active Failover with One Primary and One Secondary Resource. Remember that an Active-Active Failover uses all available resources all the time without a primary nor a secondary resource.

- the maximum backup retention period for automated backup is only 35 days. AWS backup tool can give longer duration backups. ou cannot directly download or export an automated snapshot in RDS to Amazon S3. You have to copy the automated snapshot first for it to become a manual snapshot, which you can move to an Amazon S3 bucket

- EBS volumes are only encrypted using AWS KMS

- you can attach only one EFA per EC2 instance. 

- ec2 limit of 20 per region can be icnreased by request to aws.

-  the visibility timeout isn't a guarantee against receiving a message twice. To avoid duplicate SQS messages, it is better to design your applications to be idempotent 

- If you got your certificate from a third-party CA, import the certificate into ACM or upload it to the IAM certificate store.

- AWS Global Accelerator is a service that improves the availability and performance of your applications with local or global users. It provides static IP addresses that act as a fixed entry point to your application endpoints in a single or multiple AWS Regions, such as your Application Load Balancers, Network Load Balancers or Amazon EC2 instances.AWS Global Accelerator uses the AWS global network to optimize the path from your users to your applications, improving the performance of your TCP and UDP traffic.

= although Amazon Redshift is a fully-managed data warehouse, you will still need to configure cross-region snapshot copy to ensure that your data is properly replicated to another region.

- EBS Volumes attached to stopped EC2 Instances incur cost. a stopped On-Demand EC2 Instance has no charge for a terminated EC2 instance that you have shut down

- Install the CloudWatch agent to the EC2 instances to get memory details. 

- the AWS VPN CloudHub is only for VPNs and not for VPCs. for this use Set up an AWS Transit Gateway to implement a hub-and-spoke network topology in each region that routes all traffic through a network transit center. Route traffic between VPCs and the on-premises data centers over AWS Site-to-Site VPNs.

- EC2 has pending state on start up and terminating state on scale down

- we can use run command from console to manage instances without login

- AWS trusted advisor has cost optimiation, security, fault tolerance, performance, service limits etc. Compared to this aws budgets just alerts us when reaching budget limit

- s3 has 30 days restriction on transition to s3-ia and s31z-ia. Not limit for transition to glacier. In glacier, objects remain in s3, but canot be accessed using standard s3 console. Min 30 days storage charge for s31z-ia, s3-ia and s3 intelligent tiering, even if we move it before that. s3 wont transer objects less than 128kb as they are not cost effective.

- windows ec2 support elastic fabric adapter

- Application load balancer supports weighted routing policy

- we can use iam to authenticate to rds and get temp tokens for login.

- ebs supports kms or amazon managed keys

- aurora failover: if we have replica in another az, aws flips the cname record. for aurora serverless aws re creates db in another az. if not serverless and no replica, then aws will recreate will attempt to recreate db in same az in best effort basis.

- by default cloudtrail logs are encrypted to s3 using kms

- versioning files ins s3 helps with stale cache issues

- MySQL replicas won't provide you a read replication latency of less than 1 second. RDS Read Replicas can only provide asynchronous replication in seconds and not in milliseconds. 

- Remote Desktop Protocol is allowed in the security group. By default, the server listens on TCP port 3389 and UDP port 3389.

- An Elastic IP address doesn’t incur charges as long as the following conditions are true:
The Elastic IP address is associated with an Amazon EC2 instance.The instance associated with the Elastic IP address is running. The instance has only one Elastic IP address attached to it.

- For db ssl: Force all connections to your DB instance to use SSL by setting the rds.force_ssl parameter to true. Once done, reboot your DB instance. Download the Amazon RDS Root CA certificate. Import the certificate to your servers and configure your application to use SSL to encrypt the connection to RDS

- Encryption by default is a Region-specific setting. If you enable it for a Region, you cannot disable it for individual volumes or snapshots in that Region and you can't enable it to selected EBS volumes only.

- stopping reserved ec2 instances will still incur cost.

- you can't store data in Amazon CloudFront. Technically, you only store cache data in CloudFront, but you can't host applications or web pages using this service. You have to use Amazon S3 to host the static web pages and use CloudFront as the CDN

-  RDS synchronously replicates the data to a standby instance in a different Availability Zone (AZ) that is in the same region and not in a different one.



=======================================================

- redis has data persistance.
- use amazon mq if its legacy app. Else always prefer sqs or sns for decoupling
- use EKS for cloud agnostic docker. fargate or ecs on ec2 are aws specific
- any db supported by aws can be directly migrated to aws using Database migration service. No need for schema conversion
- in SCP use deny rules to deny everything that is not needed, instead of allow rule to allow for child accouns. Beacuse master has all allows and child inherits it, so we need to all explicit deny.
- scheduled reserve instances allow us to reserve instances for speicific durations.
- for active-active config, aurora global provides multi az read replicas and so not fit. if you need read and write use dynamo global tables
- spot  block instances are spot instances that can be blocked to not be uninterruped for the duration. Reserved instances are good for 1 year to 3 year period. 
- aws recommends to less use simple scaling as with simple scaling a scaling must finish , instances launch and wait times over before next scaling. 
- We cannot create encrypted snapshot of unencrypted master. So for this, create snapshot, create new encrypted master then then create encrypted snapshots from that. 
- if you want to save data in memory of an ec2, then hibernate it. When u start hibernated instance, then ebs volume, ram, attached volumes and process are restored.
- storage gateway: block -> volume, nfs -> file.


- EFS  allows you to simultaneously share files between multiple Amazon EC2 instances across multiple AZs, regions, VPCs, and accounts as well as on-premises servers via AWS Direct Connect or AWS VPN. S3 has no heirarchial structure and not strongly consistent. 

- CF OAI is only for s3.Signed cookies and URLs are used to limit access to files but this does not stop people from circumventing CloudFront and accessing the ELB directly. So for ELB we need to update security grps to prevent access, like using a lambda.

- S3 static website is  http only. If we need https, link with CF 

- scheduled scaling can be good. But problem is we scale up even before we get loWith the EC2 launch type you can apply IAM roles at the container and task level, whereas with Fargate you can only apply at the task levelad and then load may not increase as we anticipate, leading to loss. So a better option is target trackign scaling with a much lower threhsold so that we scale early.

- For RDS,  You can create a read replica as a Multi-AZ DB instance. Amazon RDS creates a standby of your replica in another Availability Zone for failover support for the replica. Creating your read replica as a Multi-AZ DB instance is independent of whether the source database is a Multi-AZ DB instance."Deploy a read replica in a different AZ to the master DB instance" is incorrect as this does not provide high availability for the read replica

- EFS wont work with windows

- RedShift is a columnar data warehouse DB that is ideal for running long complex queries. RedShift can also improve performance for repeat queries by caching the result and returning the cached result when queries are re-run. Dashboard, visualization, and business intelligence (BI) tools that execute repeat queries see a significant boost in performance due to result caching.

- You can only apply one IAM role to a Task Definition so you must create a separate Task Definition. A Task Definition is required to run Docker containers in Amazon ECS and you can specify the IAM role (Task Role) that the task should use for permissions.With the EC2 launch type you can apply IAM roles at the container and task level, whereas with Fargate you can only apply at the task level

- snowball cannot put to glacier directly. it has to be s3.

- VPCs can be shared among multiple AWS accounts. Resources can then be shared amongst those accounts. However, to restrict access so that consumers cannot connect to other instances in the VPC the best solution is to use PrivateLink to create an endpoint for the application. The endpoint type will be an interface endpoint and it uses an NLB in the shared services

- The most resilient solution is to configure (Direct Connect) DX connections at multiple DX locations

- instance store can be useful in many cases. Like faster speed of data access, temporary data, no fear of data loss etc.


- global accelarator provides better latency than route53 geo proximity routing. route53 still happens over internet, while global accelarator finds closes edge location and then routes traffic.By default, Global Accelerator provides you with two static IP addresses that you associate with your accelerator.

- rds needs to be offline during events like Maintenance items that require a resource to be offline include required operating system or database patching.Enabling Multi-AZ, promoting a Read Replica and updating DB parameter groups are not events that take place during a maintenance window

- Instance stores offer very high performance and low latency. As long as you can afford to lose an instance, i.e. you are replicating your data, these can be a good solution for high performance/low latency requirements. Also, the cost of instance stores is included in the instance charges so it can also be more cost-effective than EBS Provisioned IOPS.

- A VPG is used to setup an AWS VPN which you can use in combination with Direct Connect to encrypt all data that traverses the Direct Connect link. This combination provides an IPsec-encrypted private connection that also reduces network costs, increases bandwidth throughput, and provides a more consistent network experience than internet-based VPN connections.

- An Aurora Replica is both a standby in a Multi-AZ configuration and a target for read traffic. Its created when we ceate aurora. no need to create separate read replicas in aurora.Aurora Replicas are independent endpoints in an Aurora DB cluster, best used for scaling read operations and increasing availability. Up to 15 Aurora Replicas can be distributed across the Availability Zones that a DB cluster spans within an AWS Region.The DB cluster volume is made up of multiple copies of the data for the DB cluster. However, the data in the cluster volume is represented as a single, logical volume to the primary instance and to Aurora Replicas in the DB cluster.

- s3 server access logging has limited capabilities to act based on event types or log file validation

- s3 is more cost effective than efs.

- In kinese, in  a case where multiple consumer applications have total reads exceeding the per-shard limits, you need to increase the number of shards in the Kinesis data stream.. You cannot increase the number of read transactions per shard. Read throttling is enabled by default for Kinesis data streams. If you’re still experiencing performance issues you must increase the number of shards.

- Amazon ElastiCache is an in-memory caching database. This is not a nonrelational key-value database.

- AWS Global Accelerator uses static IP addresses as fixed entry points for your application. So if we have to use same ip as before, global acccelarator is the correct way. 

- Alias records can be used to map the domain apex (example.com) to the Elastic Load Balancers.

- An AWS Batch multi-node parallel job is compatible with any framework that supports IP-based, internode communication, such as Apache MXNet, TensorFlow, Caffe2, or Message Passing Interface (MPI).

- For example, if you want to map an Amazon Elastic Block Store volume to an Amazon EC2 instance, you reference the logical IDs to associate the block stores with the instance. Use physical id outside of cloudfomration templates.

- You can manage a single connection for multiple VPCs or VPNs that are in the same Region by associating a Direct Connect gateway to a transit gateway. The solution involves the following components:
	A transit gateway that has VPC attachments.
	A Direct Connect gateway.
	An association between the Direct Connect gateway and the transit gateway.
	A transit virtual interface that is attached to the Direct Connect gateway.
Order is aws VPCs -> Transit gateway -> Direct Connect -> TG and DC association -> Transit virutal gatway -> Direct connecct location ->  Customer network

- if you want to trigger an async jon from an ec2, swf is not suitabel. Swf handles entire automation. here use sns or sqs.

- Lauch template -> instance. Launch config -> ASG

-  If AWS does not have capacity available a **InsufficientInstanceCapacity** error will be generated when you try to launch a new instance or restart a stopped instance.If you’ve reached the limit on the number of instances you can launch in a region you get an **InstanceLimitExceeded** error when you try to launch a new instance or restart a stopped instance

- reasons why an instance might immediately terminate:
	You’ve reached your EBS volume limit.
	An EBS snapshot is corrupt.
	The root EBS volume is encrypted and you do not have permissions to access the KMS key for decryption.
	The instance store-backed AMI that you used to launch the instance is missing a required part (an image.part.xx 	file).
    
 - for s3 its more efficiet to use an IAM policy than bucket policy
 
 - aurora gloabl tables provies inter region fail over proteciton and fast data sync. Multi-az aurora is not inter region , Also read replica in diff region is slower than global tables.
 
 -  a bucket policy that allows s3:GetObject permission with a condition, using the aws:referer key, that the get request must originate from specific webpages. using the referrer condition in a bucket policy is preferable as it is a best practice to use DNS names / URLs instead of hard-coding IPs whenever possible.
 
 - corporate sso
 	Verify that the user is authenticated by your local identity system
	Call the AWS Security Token Service (AWS STS) AssumeRole or GetFederationToken API operations to obtain temporary 		security credentials for the user
	Call the AWS federation endpoint and supply the temporary security credentials to request a sign-in token
	Construct a URL for the console that includes the token
	Give the URL to the user or invoke the URL on the user’s behalf
    
 
 - You can build a hub-and-spoke topology with AWS Transit Gateway that supports transitive routing. This simplifies the network topology and adds additional features over VPC peering. AWS Resource Access Manager can be used to share the connection with the other AWS accounts.
 
 - IAM Groups:
	Groups are collections of users and have policies attached to them.
    A group is not an identity and cannot be identified as a principal in an IAM policy.
	Use groups to assign permissions to users.
	IAM groups cannot be used to group EC2 instances.
	Only users and services can assume a role to take on permissions (not groups).
    
- To enable your Lambda function to access resources inside your private VPC, you must provide additional VPC-specific configuration information that includes VPC subnet IDs and security group IDs. 

- For RTMP CloudFront distributions files must be stored in an S3 bucket.

- Cloudtrail stores Data events(data plane ops) and Management events(contol plane ops)

- api gateway caching is for stage.

- With MySQL, authentication is handled by AWSAuthenticationPlugin—an AWS-provided plugin that works seamlessly with IAM to authenticate your IAM users.

- only The memached engine supports multiple cores and threads and large nodes.

- AWS customers are welcome to carry out security assessments or penetration tests against their AWS infrastructure without prior approval for 8 services

- Dedicated Instances are Amazon EC2 instances that run in a VPC on hardware that’s dedicated to a single customer. Your Dedicated instances are physically isolated at the host hardware level from instances that belong to other AWS accounts

- The term warm standby is used to describe a DR scenario in which a scaled-down version of a fully functional environment is always running in the cloud. 

- Amazon EBS encrypted volumes and snapshots:
	All EBS types support encryption and all instance families now support encryption.
	Not all instance types support encryption.
	Data in transit between an instance and an encrypted volume is also encrypted (data is encrypted in trans.
	You can have encrypted an unencrypted EBS volumes attached to an instance at the same time.
	Snapshots of encrypted volumes are encrypted automatically.
	EBS volumes restored from encrypted snapshots are encrypted automatically.
	EBS volumes created from encrypted snapshots are also encrypted.
    
- DynamoDB best practices include:
	Keep item sizes small.
	If you are storing serial data in DynamoDB that will require actions based on data/time use separate tables for 		days, weeks, months.
	Store more frequently and less frequently accessed data in separate tables.
	If possible compress larger attribute values.
	Store objects larger than 400KB in S3 and use pointers (S3 Object ID) in DynamoDB.
    
- If adding an instance to an ASG would result in exceeding the maximum capacity of the ASG the request will fail.Auto Scaling Groups can be edited once created (however launch configurations cannot be edited)

- Alias records are used to map resource record sets in your hosted zone to Amazon Elastic Load Balancing load balancers, API Gateway custom regional APIs and edge-optimized APIs, CloudFront Distributions, AWS Elastic Beanstalk environments, Amazon S3 buckets that are configured as website endpoints, Amazon VPC interface endpoints, and to other records in the same Hosted Zone. Cannot point to on premise server

- Amazon FSx for Windows File Server should be used for hosting SMB shares.

- ALB supports authentication from OIDC compliant identity providers such as Google, Facebook and Amazon. It is implemented through an authentication action on a listener rule that integrates with Amazon Cognito to create user pools.

- File gateway offers SMB or NFS-based access to data in Amazon S3 with local caching.

- Trusted Advisor is an online resource to help you reduce cost, increase performance, and improve security by optimizing your AWS environment. Trusted Advisor provides real time guidance to help you provision your resources following AWS best practices.

- if we freq read from s3 large data, putting it in elasticache can improve performance and speed

- The ALB operates at the HTTP and HTTPS level only (does not support TCP load balancing).

- EBS optimized instances provide dedicated capacity for Amazon EBS I/O. EBS optimized instances are designed for use with all EBS volume types.

- ASG: The health check grace period allows a period of time for a new instance to warm up before performing a health check (300 seconds by default). ASG waits for connection drainig before taking an instance off service.  ASG does not wait for the cooldown time to expire for terminating an instance. If using an ELB it is best to enable ELB health checks as otherwise EC2 status checks may show an instance as being healthy that the ELB has determined is unhealthy. In this case the instance will be removed from service by the ELB but will not be terminated by Auto Scaling.

- always check if question is asking for cost or perf improvement and decide.

- In cloudfront  Field-level encryption adds an additional layer of security that lets you protect specific data throughout system processing so that only certain applications can see it.

- With ALB and NLB IP addresses can be used to register:
	Instances in a peered VPC.
	AWS resources that are addressable by IP address and port.
	On-premises resources linked to AWS through Direct Connect or a VPN connection.
    
- You can associate an AWS Direct Connect gateway with either of the following gateways:
	A transit gateway when you have multiple VPCs in the same Region.
	A virtual private gateway.
    
- You can restore a DB instance to a specific point in time, creating a new DB instance. When you restore a DB instance to a point in time, the default DB security group is applied to the new DB instance.Restored DBs will always be a new RDS instance with a new DNS endpoint and you can restore up to the last 5 minutes

- You can use VPC Flow Logs to capture detailed information about the traffic going to and from your Elastic Load Balancer. Create a flow log for each network interface for your load balancer. There is one network interface per load balancer subnet.

- You can specify the instance store volumes for your instance only when you launch an instance. You can’t attach instance store volumes to an instance after you’ve launched it.

- in EBS, There is no direct way to change the encryption state of a volume. You can have encrypted and non-encrypted EBS volumes on a single instance.