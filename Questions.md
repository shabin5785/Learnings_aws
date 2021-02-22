**DAX will be transparent and won't require an application refactoring**, and will cache the "hot keys". So if we have a hot key problem with dynamo, putting dax will enable use to cache the hot keys and inmprove performance.

SNS, SQS and Kinesis are AWS' proprietary technologies and do not come with MQTT compatibility.

IAM Auth is not supported by ElastiCache. We can set up a rule to allow redis connection from lambda SG. But for username pass, redis auth is the way to go.

Even with a 1TB file ( which is quite small), no need for snow mobile. S3 multi part will do.

read this : https://aws.amazon.com/blogs/aws/new-application-load-balancer-sni/

deploy a critical database in RDS. To ensure High Availability, you want it to deploy on Multi-AZ environment.**What will happen when the primary instance in Multi-AZ goes down, cname will be updated to point to new.**

**Kinesis with providing a partition key in our message we can guarantee ordering for a specific sensor**, even if our stream is sharded.

S3  overwrites are eventually consistent so the latest data will not always be read. RDS is instant though.

Launch configurations are immutable meaning they cannot be updated. You have to create a new launch configuration, attach it to the ASG and then terminate old instances / launch new instances

To process these jobs, due to the unpredictable nature of their volume, and the desire to save on costs, Spot Instances are recommended over on demand.

**ElastiCache / RDS / Neptune are not serverless databases. DynamoDB is serverless**

Ingress means traffic going into your instance

ELB does not have any caching capability.

Don't make the S3 bucket public. You cannot attach IAM roles to the CloudFront distribution. **S3 buckets don't have security groups.** Here you need to use an OAI.

Redshift's internal storage does not have "tiers"
 
Even though our ASG is deployed across 3 AZ, the minimum capacity to be highly available is 2.

API Gateway Caching is possible. Adding Aurora Read Replicas would greatly increase the cost,

CloudFormation allows you to keep your infrastructure as code and re-use the best practices around your company for configuration parameters. Storing trusted advisior creds is not recommended.

**Dynamic Port Mapping is available for the Application Load Balancer. A reverse proxy solution would work but would be too much work to manage.**

**Path based routing and host based routing are only available for the Application Load Balancer (ALB).** Deploying an NGINX load balancer on EC2 would work but would suffer management and scaling issues.

Generating S3 pre-signed URLs would bypass CloudFront, therefore we should use CloudFront signed URL

Network Load Balancers expose a fixed IP to the public web, therefore allowing your application to be predictably reached using these IPs, while allowing you to scale your application behind the Network Load Balancer using an ASG. Application and Classic Load Balancers expose a fixed DNS (=URL). Finally, the ASG does not have a dynamic Elastic IPs attachment feature

the EC2 instances that are part of the ASG are the ones accessing our database layer. Finally, authorizing the entire CIDR of the ASG's subnets is overkill and would allow non ASG instances access Aurora if they were part of the same CIDR. The correct response is to add a rule to the security group attached to Aurora authorizing the EC2 instance's security group

In order to connect VPC together, your best option is to use VPC peering.

S3 access logs would not provide us the necessary information, and changing the bucket policy to require MFA would not go unnoticed. Here, and in general, to analyze any API calls made within your AWS account, you should use CloudTrail

**Elastic Beanstalk cannot manage AWS Lambda functions**

Adding the entire CIDR of the ALB would work, but wouldn't guarantee that only the ALB can access the EC2 instances that are part of the ASG. Here, the right solution is to add a rule on the ASG security group to allow incoming traffic from the security group configured for the ALB.

Scaling based on a schedule allows you to scale your application in response to predictable load changes. For example scale up on monday or weekend etc.

You can authenticate to your DB instance using AWS Identity and Access Management (IAM) database authentication. **IAM database authentication works with MySQL and PostgreSQL.** **With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token**.An authentication token is a unique string of characters that Amazon RDS generates on request. Authentication tokens are generated using AWS Signature Version 4. Each token has a lifetime of 15 minutes. You don't need to store user credentials in the database, because authentication is managed externally using IAM. You can also still use standard database authentication.
IAM database authentication provides the following benefits:
1. Network traffic to and from the database is encrypted using Secure Sockets Layer (SSL).
2. You can use IAM to centrally manage access to your database resources, instead of managing access individually on each DB instance.
3. For applications running on Amazon EC2, you can use profile credentials specific to your EC2 instance to access your database instead of a password, for greater security

Cost Explorer only helps you visualize and manage your AWS costs and usages over time. It offers a set of reports you can view data with for up to the last 13 months, forecast how much you're likely to spend for the next three months, and get recommendations for what Reserved Instances to purchase. You use Cost Explorer to identify areas that need further inquiry and see trends to understand your costs. **AWS Budgets gives you the ability to set custom budgets that alert you when your costs or usage exceed** (or are forecasted to exceed) your budgeted amount. Budgets can be tracked at the monthly, quarterly, or yearly level, and you can customize the start and end dates. You can also use AWS Budgets to set a custom reservation utilization target and receive alerts when your utilization drops below the threshold you define

When you add or remove rules, those changes are automatically applied to all instances to which you've assigned the security group

You can also install CloudWatch Agent to collect more system-level metrics from Amazon EC2 instances. Here's the list of custom metrics that you can set up:
- Memory utilization
- Disk swap utilization
- Disk space utilization
- Page file utilization
- Log collection

Amazon Kinesis Data Firehose is the easiest way to load streaming data into data stores and analytics tools. It can capture, transform, and **load streaming data into Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, and Splunk,** enabling near real-time analytics with existing business intelligence tools and dashboards you’re already using today.

You can bring part or all of your public IPv4 address range from your on-premises network to your AWS account. You continue to own the address range, but AWS advertises it on the Internet. After you bring the address range to AWS, it appears in your account as an address pool. You can create an Elastic IP address from your address pool and use it with your AWS resources, such as EC2 instances, NAT gateways, and Network Load Balancers. This is also called "Bring Your Own IP Addresses (BYOIP)".To ensure that only you can bring your address range to your AWS account, you must authorize Amazon to advertise the address range and provide proof that you own the address range.**A Route Origin Authorization (ROA) is a document that you can create through your Regional internet registry (RIR) for above.**

placement groups are more used for inter ec2 communication and not within ec2.

**Transferring data from an EC2 instance to Amazon S3, Amazon Glacier, Amazon DynamoDB, Amazon SES, Amazon SQS, or Amazon SimpleDB in the same AWS Region has no cost at all**.

Considering that the company is using a corporate Active Directory, it is best to use AWS Directory Service AD Connector for easier integration. In addition, since the roles are already assigned using groups in the corporate Active Directory, it would be better to also use IAM Roles. Take note that you can assign an IAM Role to the users or groups from your Active Directory once it is integrated with your VPC via the AWS Directory Service AD Connector.

The word ephemeral means "short-lived" or "temporary" in the English dictionary. Hence, when you see this word in AWS, always consider this as just a temporary memory or a short-lived storage. The virtual devices for instance store volumes are named as ephemeral[0-23].
The data in an instance store persists only during the lifetime of its associated instance. If an instance reboots (intentionally or unintentionally), data in the instance store persists. However, data in the instance store is lost under the following circumstances:
 - The underlying disk drive fails
 - The instance stops
 - The instance terminates
 
Amazon RDS provides metrics in real time for the operating system (OS) that your DB instance runs on. You can view the metrics for your DB instance using the console, or consume the Enhanced Monitoring JSON output from CloudWatch Logs in a monitoring system of your choice. Enhanced monitoring is better for RDS

 **In Amazon Redshift, you use workload management (WLM)** to define the number of query queues that are available, and how queries are routed to those queues for processing. WLM is part of parameter group configuration. A cluster uses the WLM configuration that is specified in its associated parameter group.
 
 When you use Amazon Redshift Enhanced VPC Routing, Amazon Redshift forces all COPY and UNLOAD traffic between your cluster and your data repositories through your Amazon VPC. By using Enhanced VPC Routing, you can use standard VPC features, such as VPC security groups, network access control lists (ACLs), VPC endpoints, VPC endpoint policies, internet gateways, and Domain Name System (DNS) servers.
 
 This particular scenario tests your understanding of EBS, EFS, and S3. In this scenario, there is a fleet of On-Demand EC2 instances that stores file documents from the users to one of the attached EBS Volumes. The system performance is quite slow because the architecture doesn't provide the EC2 instances a parallel shared access to the file documents.Remember that an EBS Volume can be attached to one EC2 instance at a time, hence, no other EC2 instance can connect to that EBS Provisioned IOPS Volume. Take note as well that the type of storage needed here is a "file storage" which means that S3 (Option 1) is not the best service to use because it is mainly used for "object storage", and S3 does not provide the notion of "folders" too. EFS is the best solution
 
although the use of Secrets Manager in securing sensitive data in ECS is valid, using an IAM Role is a more suitable choice over a resource-based policy for the Amazon ECS task execution role.

Amazon API Gateway provides throttling at multiple levels including global and by service call.

CloudFront signed URLs and signed cookies provide the same basic functionality: they allow you to control who can access your content. If you want to serve private content through CloudFront and you're trying to decide whether to use signed URLs or signed cookies, consider the following:
Use signed URLs for the following cases:
-You want to use an RTMP distribution. Signed cookies aren't supported for RTMP distributions.
-You want to restrict access to individual files, for example, an installation download for your application.
-Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.
Use signed cookies for the following cases:
-You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of a website.
-You don't want to change your current URLs.

**ssd for small random iops and hdd for sequential large iops**

Amazon RDS Multi-AZ deployments provide enhanced availability and durability for Database (DB) Instances, making them a natural fit for production database workloads. When you provision a Multi-AZ DB Instance, Amazon RDS automatically creates a primary DB Instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ). Each AZ runs on its own physically distinct, independent infrastructure, and is engineered to be highly reliable.

Amazon Aurora typically involves a cluster of DB instances instead of a single instance. Each connection is handled by a specific DB instance. When you connect to an Aurora cluster, the host name and port that you specify point to an intermediate handler called an endpoint. Aurora uses the endpoint mechanism to abstract these connections. Thus, you don't have to hardcode all the hostnames or write your own logic for load-balancing and rerouting connections when some DB instances aren't available.**For certain Aurora tasks, different instances or groups of instances perform different roles.Using endpoints, you can map each connection to the appropriate instance or group of instances based on your use case**

**In Cloudfront,you can set up an origin failover by creating an origin group with two origins with one as the primary origin and the other as the second origin which CloudFront automatically switches to when the primary origin fails.**

only Convertible Reserved Instances can be exchanged for other Convertible Reserved Instances

**AWs shield for protect against ddos attack**

Cold HDD volumes provide low-cost magnetic storage that defines performance in terms of throughput rather than IOPS. With a lower throughput limit than Throughput Optimized HDD, this is a good fit ideal for large, sequential cold-data workloads. Throughput Optimized HDD is primarily used for frequently accessed, throughput-intensive workloads. In this scenario, Cold HDD perfectly fits the requirement as it is used for their infrequently accessed data and provides the lowest cost, unlike Throughput Optimized HDD

If you're using the AWS Lambda compute platform, you must choose one of the following deployment configuration types to specify how traffic is shifted from the original AWS Lambda function version to the new AWS Lambda function version:
-Canary: Traffic is shifted in two increments. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Lambda function version in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment.
-Linear: Traffic is shifted in equal increments with an equal number of minutes between each increment
-All at once

in question of vpc check if peering is being asked. Peering is not possible in vpc. 

Using Amazon CloudWatch alarm actions, you can create alarms that automatically stop, terminate, reboot, or recover your EC2 instances. You can use the stop or terminate actions to help you save money when you no longer need an instance to be running. You can use the reboot and recover actions to automatically reboot those instances or recover them onto new hardware if a system impairment occurs.

The best way to implement a bastion host is to create a small EC2 instance which should only have a security group from a particular IP address for maximum security. This will block any SSH Brute Force attacks on your bastion host. It is also recommended to use a small instance rather than a large one because this host will only act as a jump server to connect to other instances in your VPC and nothing else.

AWS IoT Core is a managed cloud service that lets connected devices easily and securely interact with cloud applications and other devices

For EBS since the instance is using a RAID configuration, the snapshot process is different. You should stop all I/O activity of the volumes before creating a snapshot. So for backup
1. Stop all applications from writing to the RAID array.
2. Flush all caches to the disk.
3. Confirm that the associated EC2 instance is no longer writing to the RAID array by taking actions such as freezing the file system, unmounting the RAID array, or even shutting down the EC2 instance.
4. After taking steps to halt all disk-related activity to the RAID array, take a snapshot of each EBS volume in the array.

By default, all data stored by AWS Storage Gateway in S3 is encrypted server-side with Amazon S3-Managed Encryption Keys (SSE-S3).  **RDS doesnt have default encryption and we need to enable it**

**We cannot set priority in SQS items**. So for that use case create separate queues for high priority items

Perfect Forward Secrecy is a feature that provides additional safeguards against the eavesdropping of encrypted data, through the use of a unique random session key. This prevents the decoding of captured data, even if the secret long-term key is compromised.**CloudFront and Elastic Load Balancing are the two AWS services that support Perfect Forward Secrecy.**

If the shard iterator expires immediately before you can use it, this might indicate that the DynamoDB table used by Kinesis does not have enough capacity to store the lease data. This situation is more likely to happen if you have a large number of shards. To solve this problem, increase the write capacity assigned to the shard table.

OS patching of ec2 is a user responsibility

EBS data can persist ouside of an instance. and support live config updates.when you create an EBS volume in an Availability Zone, it is automatically replicated within that zone only. EBS volumes can only be attached to an EC2 instance in the same Availability Zone.the EBS Volume snapshots are  sent to Amazon S3.

Questions about which services an accomplish a task, check if a combination is being asked. Like monitor a task and send notifications. So cloudwatch + SNS. even though ses can send emails, its not the combiation answer here.

 Amazon API Gateway has no minimum fees or startup costs. You pay only for the API calls you receive and the amount of data transferred out.
 
**Classic Load Balancer does not support Server Name Indication (SNI). You have to use an Application Load Balancer instead or a CloudFront web distribution to allow the SNI feature.**
 
Amazon Simple Queue Service (SQS) and Amazon Simple Workflow Service (SWF) are the services that you can use for creating a decoupled architecture in AWS.there is no such thing as Amazon Simple Decoupling Service.

An elastic network interface (ENI) is a logical networking component in a VPC that represents a virtual network card. You can attach a network interface to an EC2 instance in the following ways:
1. When it's running (hot attach)
2. When it's stopped (warm attach)
3. When the instance is being launched (cold attach).

**S3 can send event notifications to SQS, SNS and Lambda**

dynamodb is a good choice to store s3 objects metadata

80 TB Snowball appliance and 100 TB Snowball Edge appliance only have 72 TB and 83 TB of usable capacity respectively.

AWS Security Token Service (AWS STS) is the service that you can use to create and provide trusted users with temporary security credentials that can control access to your AWS resources

You can use **Amazon Data Lifecycle Manager (Amazon DLM)** to automate the creation, retention, and deletion of snapshots taken to back up your Amazon EBS volumes

In Auto Scaling, the following statements are correct regarding the cooldown period:
1. It ensures that the Auto Scaling group does not launch or terminate additional EC2 instances before the previous scaling activity takes effect.
2. Its default value is 300 seconds.
3. It is a configurable setting for your Auto Scaling group.

Redshit is used for data warehousing and glacier is not suited for this.

by default, Network ACL allows all inbound and outbound IPv4 traffic, then there is no point of explicitly allowing the port in the Network ACL. Security Groups, on the other hand, does not allow incoming traffic by default, unlike Network ACL.

 You can retrieve a unique Amazon Cognito identifier (identity ID) for your end user immediately if you're allowing unauthenticated users or after you've set the login tokens in the credentials provider if you're authenticating users.
 
Lambda by itself can be async exectuted. Depending on situation this is more cost effective than Lambda + Step.

**pre warming a cluster of instances is not efective as we cannot predict the load beforehand.**

**maximum of 20 instances limit is set per region and not per Availability Zone**. This can be increased after submitting a request form to AWS.

an instance store-backed instance can only be rebooted or terminated and its data will be erased if the EC2 instance is terminated. only EBS-backed instances can be stopped and restarted,

Restarting instances might move ec2 instances to new physical host. 

Provisioned capacity ensures that your retrieval capacity for expedited retrievals is available when you need it. Each unit of capacity provides that at least three expedited retrievals can be performed every five minutes and provides up to 150 MB/s of retrieval throughput. You should purchase provisioned retrieval capacity if your workload requires highly reliable and predictable access to a subset of your data in minutes. Without provisioned capacity Expedited retrievals are accepted, except for rare situations of unusually high demand. However, if you require access to Expedited retrievals under all circumstances, you must purchase provisioned retrieval capacity.

AWS Lambda automatically monitors functions on your behalf, reporting metrics through Amazon CloudWatch. These metrics include total invocation requests, latency, and error rates. The throttles, Dead Letter Queues errors and Iterator age for stream-based invocations are also monitored.

by default, Auto Scaling is not enabled in a DynamoDB table which is created using the AWS CLI.

The following VPC peering connection configurations are not supported.
1. Overlapping CIDR Blocks
2. Transitive Peering
3. Edge to Edge Routing Through a Gateway or Private Connection

The revoke-security-group-ingress command removes one or more ingress rules from a security group.

You can use Route 53 health checking to configure active-active and active-passive failover configurations. You configure active-active failover using any routing policy (or combination of routing policies) other than failover, and you configure active-passive failover using the failover routing policy.

Active-Active Failover : Use this failover configuration when you want all of your resources to be available the majority of the time. When a resource becomes unavailable, Route 53 can detect that it's unhealthy and stop including it when responding to queries.In active-active failover, all the records that have the same name, the same type (such as A or AAAA), and the same routing policy 

Active-Passive Failover : Use an active-passive failover configuration when you want a primary resource or group of resources to be available the majority of the time and you want a secondary resource or group of resources to be on standby in case all the primary resources become unavailable. When responding to queries, Route 53 includes only the healthy primary resources. If all the primary resources are unhealthy, Route 53 begins to include only the healthy secondary resources in response to DNS queries.

**RDS read replicas provide elasticity to apps. (Easy to scale out)**

Every subnet that you create is automatically associated with the main route table for the VPC.

allowed block size in VPC is between a /16 netmask (65,536 IP addresses) and /28 netmask (16 IP addresses)

**When the instance state is stopping, you will not billed if it is preparing to stop however, you will still be billed if it is just preparing to hibernate.**

signed cookies feature is primarily used if you want to provide access to multiple restricted files

**when a Reserved Instance expires, any instances that were covered by the Reserved Instance are billed at the on-demand price which costs significantly higher.** Since the application is already decommissioned, there is no point of keeping the unused instances. It is also possible that there are associated Elastic IP addresses, which will incur charges if they are associated with stopped instances. So better termiante them earlier.

 Amazon EFS provides the scale and performance required for big data applications that require high throughput to compute nodes coupled with read-after-write consistency and low-latency file operations.Amazon EFS is a fully-managed service that makes it easy to set up and scale file storage in the Amazon Cloud.Amazon EFS file systems can automatically scale from gigabytes to petabytes of data without needing to provision storage.
 
Although S3 can handle concurrent connections from multiple EC2 instances, it does not have the ability to provide low-latency file operations, which is required in this scenario.

An Amazon S3 Glacier (Glacier) vault can have one resource-based vault access policy and one Vault Lock policy attached to it.**A Vault Lock policy is a vault access policy that you can lock. Using a Vault Lock policy can help you enforce regulatory and compliance requirements. Amazon S3 Glacier** provides a set of API operations for you to manage the Vault Lock policies. This tool is great for storing exteme confidential data.

you can use Amazon EMR to transform and move large amounts of data into and out of other AWS data stores and databases such as Amazon Simple Storage Service (Amazon S3) and Amazon DynamoDB.

**there are no additional charges for creating and using the VPC itself**. So creating vpc cost zero. But creating and using resources inside it costs money.

**SWF is a fully-managed state tracker and task coordinator service. It does not provide serverless orchestration to multiple AWS resources.AWS Step Functions provides serverless orchestration for modern applications.**

SID: statement ID

**Amazon S3 Select is designed to help analyze and process data within an object in Amazon S3 buckets**, faster and cheaper. It works by providing the ability to retrieve a subset of data from an object in Amazon S3 using simple SQL expressions.Amazon Redshift also includes Redshift Spectrum, allowing you to directly run SQL queries against exabytes of unstructured data in Amazon S3. 

When you upload your data in S3, your objects are redundantly stored on multiple devices across multiple facilities within the region only, where you created the bucket. **Hence, if there is an outage on the entire region, your S3 bucket will be unavailable**if you do not enable Cross-Region Replication, which should make your data available to another region.

You can configure Amazon Redshift to copy snapshots for a cluster to another region. To configure cross-region snapshot copy, you need to enable this copy feature for each cluster and configure where to copy snapshots and how long to keep copied automated snapshots in the destination region.**Although Amazon Redshift is a fully-managed data warehouse, you will still need to configure cross-region snapshot copy to ensure that your data is properly replicated to another region**

To collect logs from your Amazon EC2 instances and on-premises servers into CloudWatch Logs, AWS offers both a new unified CloudWatch agent, and an older CloudWatch Logs agent. **It is recommended to use the unified CloudWatch agent**

for standard queues, the visibility timeout isn't a guarantee against receiving a message twice. To avoid duplicate SQS messages, it is better to design your applications to be idempotent . **Amazon SWF provides useful guarantees around task assignment. It ensures that a task is never duplicated and is assigned only once. So its great to use this for decoupled tasks needing only once guarantee.**

AWS RDS enhanced metrics: RDS child processes, and OS processes.

static AnyCast IP address is primarily used for AWS Global Accelerator and not for security group configurations.We can give security group id as source when defining rules.

**An Elastic Fabric Adapter (EFA) is a network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications**

If you got your certificate from a third-party CA, import the certificate into ACM or upload it to the IAM certificate store. 

**AWS Global Accelerator is a service that improves the availability and performance of your applications with local or global users.** It provides static IP addresses that act as a fixed entry point to your application endpoints in a single or multiple AWS Regions, such as your Application Load Balancers, Network Load Balancers or Amazon EC2 instances.**Although the Amazon CloudFront service uses edge locations, it doesn't have the capability to route the traffic to the closest edge location via an Anycast static IP address.**

You can use AWS X-Ray to trace and analyze user requests as they travel through your Amazon API Gateway APIs to the underlying services. 

By default, each workflow execution can run for a maximum of 1 year in Amazon SWF. **Amazon SWF does not take any special action if a workflow execution is idle for an extended period of time.** Idle executions are subject to the timeouts that you configure. For example, if you have set the maximum duration for an execution to be 1 day, then an idle execution will be timed out if it exceeds the 1 day limit. Idle executions are also subject to the Amazon SWF limit on how long an execution can run (1 year).

AWS Glue is a fully managed extract, transform, and load (ETL) service that makes it easy for customers to prepare and load their data for analytics. **It does not provide the ability to process your data in real-time, unlike Kinesis**.

Amazon S3 offers access policy options broadly categorized as resource-based policies and user policies. Access policies you attach to your resources (buckets and objects) are referred to as resource-based policies.For example, bucket policies and access control lists (ACLs) are resource-based policies. You can also attach access policies to users in your account. These are called user policies. You may choose to use resource-based policies, user policies, or some combination of these to manage permissions to your Amazon S3 resources.

Amazon EBS can deliver performance for workloads that require the lowest-latency access to data from a single EC2 instance. You can also increase EBS storage for up to 16TB or add new volumes for additional storage.

**To ensure that a Classic Load Balancer stops sending requests to instances that are de-registering or unhealthy while keeping the existing connections open, use connection draining.** This enables the load balancer to complete in-flight requests made to instances that are de-registering or unhealthy.

Amazon Athena is just an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. It is not a storage service where you can store the results processed by the consumers

**Kinesis can store their results using an AWS service such as Amazon DynamoDB, Amazon Redshift, or Amazon S3.**

Glacier has a management console which you can use to create and delete vaults. **However, you cannot directly upload archives to Glacier by using the management console**

Since the Spot instance has been running for more than an hour, which is past the first instance hour, this means that you will be charged from the time it was launched till the time it was terminated by AWS. The computation for your 90 minute usage would be $0.04 (60 minutes) + $0.02 (30 minutes) = $0.06.

if the private key that you are using has a file permission of 0777, then it will throw an "Unprotected Private Key File" error and not a "Server refused our key" error.

**AWS Trusted Advisor is an online tool that provides you real-time guidance to help you provision your resources following AWS best practices.** It inspects your AWS environment and makes recommendations for saving money, improving system performance and reliability, or closing security gaps.

Amazon EC2 Auto Scaling supports the following types of scaling policies:
1. Target tracking scaling - Increase or decrease the current capacity of the group based on a target value for a specific metric. This is similar to the way that your thermostat maintains the temperature of your home – you select a temperature and the thermostat does the rest.
2. Step scaling - Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach.
3. Simple scaling - Increase or decrease the current capacity of the group based on a single scaling adjustment.

**You can define the tags on the UAT and production EC2 instances and add a condition to the IAM policy which allows access to specific tags.**
 
**AWS Database Migration Service helps you migrate databases to AWS quickly and securely**. The source database remains fully operational during the migration, minimizing downtime to applications that rely on the database. For hetrogenous db, migrations is a two step process. F**irst use the AWS Schema Conversion Tool to convert the source schema and code to match that of the target database, and then use the AWS Database Migration Service to migrate data from the source database to the target database.**

**You can use Run Command from the console to configure instances without having to login to each instance.**

If you have an Amazon Aurora Replica in the same or a different Availability Zone, when failing over, Amazon Aurora flips the canonical name record (CNAME). If you are running Aurora Serverless and the DB instance or AZ become unavailable, Aurora will automatically recreate the DB instance in a different AZ. If you do not have an Amazon Aurora Replica (i.e. single instance) and are not running Aurora Serverless, Aurora will attempt to create a new DB Instance in the same Availability Zone as the original instance.

 IAM roles are global service that are available to all regions
 
 **By default, CloudTrail event log files are encrypted using Amazon S3 server-side encryption (SSE).** You can also choose to encrypt your log files with an AWS Key Management Service (AWS KMS) key.
 
 You manage your DB engine configuration through the use of parameters in a DB parameter group. DB parameter groups act as a container for engine configuration values that are applied to one or more DB instances.
 
CNAME records cannot be created for your zone apex

 SDD volumes which are more suitable for small, random I/O operations. HDD large frequent

**It is important to note that for Amazon S3 and DynamoDB service, you have to create a gateway endpoint and then use an interface endpoint for other services.**

 **you only store keys in CloudHSM and not passwords.**
 
 Elastic Network Adapters (ENAs) provide traditional IP networking features that are required to support VPC networking. **An Elastic Fabric Adapter (EFA) is simply an Elastic Network Adapter (ENA) with added capabilities.** It provides all of the functionality of an ENA, with additional OS-bypass functionality.The OS-bypass capabilities of EFAs are not supported on Windows instances. **If you attach an EFA to a Windows instance, the instance functions as an Elastic Network Adapter, without the added EFA capabilities.**
 
 AWS CloudTrail increases visibility into your user and resource activity by recording AWS Management Console actions and API calls. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred.
 
**Objects must be stored at least 30 days in the current storage class before you can transition them to STANDARD_IA or ONEZONE_IA.**
 
 Amazon FSx provides fully managed third-party file systems. Amazon FSx provides you with two file systems to choose from: Amazon FSx for Windows File Server for Windows-based applications and Amazon FSx for Lustre for compute-intensive workloads.
 
 Application Load Balancers support Weighted Target Groups routing
 
**In EC2-Classic, your EC2 instance receives a private IPv4 address from the EC2-Classic range each time it's started. In EC2-VPC on the other hand, your EC2 instance receives a static private IPv4 address from the address range of your default VPC.**

You can connect your VPC to remote networks by using a VPN connection which can be Direct Connect, IPsec VPN connection, AWS VPN CloudHub, or a third party software VPN appliance. 

You can use path conditions to define rules that forward requests to different target groups based on the URL in the request (also known as path-based routing).

Storage optimized instances are designed for workloads that require high, sequential read and write access to very large data sets on local storage. They are optimized to deliver tens of thousands of low-latency, random I/O operations per second (IOPS) to applications.

An Elastic IP address doesn’t incur charges as long as the following conditions are true:
	-The Elastic IP address is associated with an Amazon EC2 instance.
	-The instance associated with the Elastic IP address is running.
	-The instance has only one Elastic IP address attached to it.

 An overlay multicast is a method of building IP level multicast across a network fabric supporting unicast IP routing, such as Amazon Virtual Private Cloud (Amazon VPC)
 
 AWS hosts a variety of public datasets that anyone can access for free.
 
**By using Docker with Elastic Beanstalk, you have an infrastructure that automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring.**
 
 The main issue is the slow upload time of the video objects to Amazon S3. To address this issue, you can use Multipart upload in S3 to improve the throughput. It allows you to upload parts of your object in parallel thus, decreasing the time it takes to upload big objects. Each part is a contiguous portion of the object's data.
 
**RDS synchronously replicates the data to a standby instance in a different Availability Zone (AZ) that is in the same region and not in a different one.**
 
If your Spot instance is terminated or stopped by Amazon EC2 in the first instance hour, you will not be charged for that usage. However, if you terminate the instance yourself, you will be charged to the nearest second.

To control the versions of files that are served from your distribution, you can either invalidate files or give them versioned file names. **If you want to update your files frequently, AWS recommends that you primarily use file versioning. helps with cache as well.**

AWS Elastic Beanstalk stores your application files and optionally, server log files in Amazon S3

It is recommended that you launch the number of instances that you need in the placement group in a single launch request and that you use the same instance type for all instances in the placement group. If you try to add more instances to the placement group later, or if you try to launch more than one instance type in the placement group, you increase your chances of getting an insufficient capacity error.If you receive a capacity error when launching an instance in a placement group that already has running instances, stop and start all of the instances in the placement group, and try the launch again. Restarting the instances may migrate them to hardware that has capacity for all the requested instances.

AWS Key Management Service (KMS) is a multi-tenant, managed service that allows you to use and manage encryption keys. **AWS CloudHSM is a cloud-based hardware security module (HSM)** that enables you to easily generate and use your own encryption keys on the AWS Cloud. Both services offer a high level of security for your cryptographic keys.

Take note that an **egress-only Internet gateway is for use with IPv6 traffic only.** To enable outbound-only Internet communication over IPv4, use a NAT gateway instead.

although the Amazon Elastic File System (EFS) service can be used for HPC applications, it doesn't natively work with Amazon S3. It doesn't have the capability to easily process your S3 data with a high-performance POSIX interface, unlike Amazon FSx for Lustre.

**Amazon Route 53’s DNS services does not support DNSSEC at this time**

**Take note that in CF all of the sections here are optional, except for Resources, which is the only one required.**

There are 2 ways to use SSL to connect to your SQL Server DB instance:
	- Force SSL for all connections — this happens transparently to the client, and the client doesn't have to do any work to use SSL.
	- Encrypt specific connections — this sets up an SSL connection from a specific client computer, and you must do work on the client to encrypt connections.
    Just forcing port 443 is not enough.

---------------------

IAM roles for ECS tasks enabled you to secure your infrastructure by assigning an IAM role directly to the ECS task rather than to the EC2 container instance. This means you can have one task that uses a specific IAM role for access to S3 and one task that uses an IAM role to access DynamoDB.With IAM roles for EC2 instances you assign all of the IAM policies required by tasks in the cluster to the EC2 instances that host the cluster. This does not allow the secure separation requested

The CloudFormation API accepts a **ResourceTypes parameter**. In your API call, you specify which types of resources can be created or updated. The cloudformation:**TemplateURL**, lets you specify where the CloudFormation template for a stack action, such as create or update, resides and enforce that it be used. You can ensure that every CloudFormation stack has a stack policy associated with it upon creation with the **StackPolicyURL** condition.

 ALB supports HTTP/HTTPS listeners. The NLB only supports TCP, TLS, UDP, TCP_UDP
 
API Gateway scales up to the default throttling limit of 10,000 requests per second, and can burst past that up to 5,000 RPS. Throttling is used to protect back-end instances from traffic spikes

 Spread placement groups are recommended for applications that have a small number of critical instances that should be kept separate from each other. Launching instances in a spread placement group reduces the risk of simultaneous failures that might occur when instances share the same underlying hardware.  cluster placement group is a logical grouping of instances within a single Availability Zone
 
 Amazon DynamoDB defaults to eventual consistency. This may result in stale data being returned in some cases. To prevent this from happening you can request strongly consistent reads using an SDK parameter. This can be written into your application.
 
 Amazon S3 does not support a hierarchical structure. Though you can create folders within buckets, these are actually just pointers to groups of objects. The structure is flat in Amazon S3. Also, the consistency model of Amazon S3 is read-after-write for PUTS of new objects, but only eventual consistency for overwrite PUTS and DELETES. **This does not support the requirement for strong consistency**
 
 There two main ways you can increase performance on an Amazon RDS database are 1) scale up to a larger RDS instance type with more CPU/RAM, and 2) use RDS read replicas to offload read traffic from the master database instance.
 
**System status checks** detect (StatusCheckFailed_System) problems with your instance that require AWS involvement to repair whereas **Instance status checks** (StatusCheckFailed_Instance) detect problems that require your involvement to repair

The action to recover the instance is only supported on specific instance types and can be used only with **StatusCheckFailed_System**

An Amazon API Gateway is a collection of resources and methods that are integrated with back-end HTTP endpoints, Lambda function or other AWS services. API Gateway handles all of the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls. Throttling can be configured at multiple levels including Global and Service Call

You can only apply one IAM role to a Task Definition so you must create a separate Task Definition.. A Task Definition is required to run Docker containers in Amazon ECS and you can specify the IAM role (Task Role) that the task should use for permissions

RedShift can also improve performance for repeat queries by caching the result and returning the cached result when queries are re-run. Dashboard, visualization, and business intelligence (BI) tools that execute repeat queries see a significant boost in performance due to result caching


You can passthrough encrypted traffic with an NLB and terminate the SSL on the EC2 instances, so this is a valid answer. You can use a HTTPS listener with an ALB and install certificates on both the ALB and EC2 instances. This does not use passthrough, instead it will terminate the first SSL connection on the ALB and then re-encrypt the traffic and connect to the EC2 instances.

You cannot encrypt an existing DB, you need to create a snapshot, copy it, encrypt the copy, then build an encrypted DB from the snapshot You can encrypt your Amazon RDS instances and snapshots at rest by enabling the encryption option for your Amazon RDS DB instance .Data that is encrypted at rest includes the underlying storage for a DB instance, its automated backups, Read Replicas, and snapshots .A Read Replica of an Amazon RDS encrypted instance is also encrypted using the same key as the master instance when both are in the same region .If the master and Read Replica are in different regions, you encrypt using the encryption key for that region


DB can be run in EBS hdd and ssd. hdd is cheaper in this case.

EC2 enhanced networking can bring down latency.

Glacier can be used to copy or archive files. Glacier integrates with versioning to allow you to choose policies for transitioning current and previous versions to a Glacier archive .Versioning stores all versions of an object (including all writes and even if an object is deleted). With versioning you have to pay for the extra consumed space but there are no data egress costs. Versioning protects against accidental object/data deletion or overwrites
CRR is an Amazon S3 feature that automatically replicates data across AWS Regions. However, there are data egress costs to consider when copying data across regions and you have to pay for 2 copies of the data 

AWS Directory Service Simple AD is an inexpensive Active Directory-compatible service with common directory features. It is a fully cloud-based solution and does not integrate with an on-premises Active Directory service
**You map the groups in AD to IAM Roles, not IAM users or groups**

Aurora Replicas are independent endpoints in an Aurora DB cluster, best used for scaling read operations and increasing availability. Up to 15 Aurora Replicas can be distributed across the Availability Zones that a DB cluster spans within an AWS Region. To increase availability, you can use Aurora Replicas as failover targets. That is, if the primary instance fails, an Aurora Replica is promoted to the primary instance

If you are installing MySQL on an EC2 instance you cannot enable read replicas or multi-AZ. Instead you would need to use Amazon RDS with a MySQL DB engine to use these features

You can suspend and then resume one or more of the scaling processes for your Auto Scaling group. This can be useful when you want to investigate a configuration problem or other issue with your web application and then make changes to your application, without invoking the scaling processes. You can manually move an instance from an ASG and put it in the standby state .Instances in standby state are still managed by Auto Scaling, are charged as normal, and do not count towards available EC2 instance for workload/application use. Auto scaling does not perform health checks on instances in the standby state. Standby state can be used for performing updates/changes/troubleshooting etc. without health checks being performed or replacement instances being launched


Proxy protocol for TCP/SSL carries the source (client) IP/port information. The Proxy Protocol header helps you identify the IP address of a client when you have a load balancer that uses TCP for back-end connections. You need to ensure the client doesn’t go through a proxy or there will be multiple proxy headers. You also need to ensure the EC2 instance’s TCP stack can process the extra information. The back-end and front-end listeners must be configured for TCP.

Launch templates enable you to store launch parameters so that you do not have to specify them every time you launch an instance. When you launch an instance using the Amazon EC2 console, an AWS SDK, or a command line tool, you can specify the launch template to use

The following are a few reasons why an instance might immediately terminate:
- You've reached your EBS volume limit
- An EBS snapshot is corrupt
- The root EBS volume is encrypted and you do not have permissions to access the KMS key for decryption
- The instance store-backed AMI that you used to launch the instance is missing a required part (an image.part.xx file)

For better performance, lower latency and lower costs the buckets should be created in the region that is closest to the client's customers

Information is sent by the ELB to CloudWatch **every 1 minute when requests are active**. Can be used to trigger SNS notifications . Access Logs are disabled by default. 

When request submissions exceed the steady-state request rate and burst limits, API Gateway fails the limit-exceeding requests and returns 429 Too Many Requests error responses to the client

AWS OpsWorks for Chef Automate is a fully-managed configuration management service that hosts Chef Automate, a suite of automation tools from Chef 

Amazon EMR is a web service that enables businesses, researchers, data analysts, and developers to easily and cost-effectively process vast amounts of data. EMR utilizes a hosted Hadoop framework running on Amazon EC2 and Amazon S3


ECS Clusters are a logical grouping of container instances the you can place tasks on .Clusters can contain tasks using BOTH the Fargate and EC2 launch type .Each container instance may only be part of one cluster at a time . Clusters are region specific.

**Trusted Advisor** is an online resource to help you reduce cost, increase performance, and improve security by optimizing your AWS environment. Trusted Advisor provides real time guidance to help you provision your resources following AWS best practices. AWS Trusted Advisor offers a Service Limits check (in the Performance category) that displays your usage and limits for some aspects of some services

**Systems Manager** provides a unified user interface so you can view operational data from multiple AWS services and allows you to automate operational tasks across your AWS resources

You should implement an AWS Direct Connect connection to the closest region. You can then use Direct Connect gateway to create private virtual interfaces (VIFs) to each AWS region. Direct Connect gateway provides a grouping of Virtual Private Gateways (VGWs) and Private Virtual Interfaces (VIFs) that belong to the same AWS account and enables you to interface with VPCs in any AWS Region (except AWS China Region). You can share a private virtual interface to interface with more than one Virtual Private Cloud (VPC) reducing the number of BGP sessions required

Internet facing ELB nodes have public IPs .Both types of ELB route traffic to the private IP addresses of EC2 instances. For public facing ELBs you must have one public subnet in each AZ where the ELB is defined.

AWS Step Functions orchestrates serverless workflows including coordination, state, and function chaining as well as combining long-running executions not supported within Lambda execution limits by breaking into multiple steps or by calling workers running on Amazon Elastic Compute Cloud (Amazon EC2) instances or on-premises

You can specify which subnets Auto Scaling will launch new instances into. Auto Scaling will try to distribute EC2 instances evenly across AZs. If only one subnet has EC2 instances running in it the first thing to check is that you have added all relevant subnets to the configuration

**We cannot deploy an app using Cloudformation. Only provision resources**

Creating vpc we can use create a virtual private gateway to connect vpc to our network

You can have encrypted an unencrypted EBS volumes attached to an instance at the same time. Snapshots of encrypted volumes are encrypted automatically .EBS volumes restored from encrypted snapshots are encrypted automatically. EBS volumes created from encrypted snapshots are also encrypted

CloudFront is used as the public endpoint for API Gateway and provides reduced latency and distributed denial of service protection through the use of CloudFront

When an EBS volume is encrypted with a custom key you must share the custom key with the PROD account. You also need to modify the permissions on the snapshot to share it with the PROD account. The PROD account must copy the snapshot before they can then create volumes from the snapshot

An Interface endpoint uses AWS PrivateLink and is an elastic network interface (ENI) with a private IP address that serves as an entry point for traffic destined to a supported service. Using PrivateLink you can connect your VPC to supported AWS services, services hosted by other AWS accounts (VPC endpoint services), and supported AWS Marketplace partner services

Auto Scaling helps to reduce the impact of failure by launching replacement instances.

ALB supports authentication from OIDC compliant identity providers such as Google, Facebook and Amazon. It is implemented through an authentication action on a listener rule that integrates with Amazon Cognito to create user pools SAML can be used with Amazon Cognito but this is not the only option.

The AWS Storage Gateway Volume Gateway in cached volume mode is a block-based (not file-based) solution so you cannot mount the storage with the SMB or NFS protocols With Cached Volume mode 

To enable your Lambda function to access resources inside your private VPC, you must provide additional VPC-specific configuration information that includes VPC subnet IDs and security group IDs. AWS Lambda uses this information to set up elastic network interfaces (ENIs) that enable your function

AWS CloudFormation provides two methods for updating stacks: **direct update or creating and executing change sets**. When you directly update a stack, you submit changes and AWS CloudFormation immediately deploys them. Use direct updates when you want to quickly deploy your updates. With change sets, you can preview the changes AWS CloudFormation will make to your stack, and then decide whether to apply those changes

Lambda automatically monitors Lambda functions and reports metrics through CloudWatch. Lambda tracks the number of requests, the latency per request, and the number of requests resulting in an error.

AWS customers are welcome to carry out security assessments or penetration tests against their AWS infrastructure without prior approval for 8 services

The Network Load Balancer operates at the connection level (Layer 4), routing connections to targets – Amazon EC2 instances, containers and IP addresses based on IP protocol data. It is architected to handle millions of requests/sec, sudden volatile traffic patterns and provides extremely low latencies. It provides high throughput and extremely low latencies and is designed to handle traffic as it grows and can load balance millions of requests/second. NLB also supports load balancing to multiple ports on an instance

The CLB operates using the TCP, SSL, HTTP and HTTPS protocols. The ALB operates at the HTTP and HTTPS level only (does not support TCP load balancing)

Throughput Optimized HDD (st1) volumes are recommended for streaming workloads requiring consistent, fast throughput at a low price. Examples include Big Data warehouses and Log Processing. You cannot use these volumes as a boot volume

Simple AD is an inexpensive Active Directory-compatible service with common directory features. It is a standalone, fully managed, directory on the AWS cloud and is generally the least expensive option. It is the best choice for less than 5000 users and when you don’t need advanced AD features.

Active Directory Service for Microsoft Active Directory is the best choice if you have more than 5000 users and/or need a trust relationship set up. It provides advanced AD features that you don’t get with SimpleAD.

You can use block device mapping to specify additional EBS volumes or instance store volumes to attach to an instance when it's launched. You can also attach additional EBS volumes to a running instance

However, EFS is the wrong answer as the solution asks to maximize availability and durability. The File Gateway backs off of Amazon S3 which has much higher availability and durability than EFS which is why it is the best solution for this scenario.

Default security groups have inbound allow rules (allowing traffic from within the group)