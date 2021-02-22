- Following are the unsupported life cycle transitions for S3 storage classes - Any storage class to the S3 Standard storage class. Any storage class to the Reduced Redundancy storage class. The S3 Intelligent-Tiering storage class to the S3 Standard-IA storage class. The S3 One Zone-IA storage class to the S3 Standard-IA or S3 Intelligent-Tiering storage classes

- When you provision a Multi-AZ DB Instance, Amazon RDS automatically creates a primary DB Instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ). Multi-AZ spans at least two Availability Zones within a single region. Read replicas can be within an Availability Zone, Cross-AZ, or Cross-Region.

- With target tracking scaling policies, you select a scaling metric and set a target value. Amazon EC2 Auto Scaling creates and manages the CloudWatch alarms that trigger the scaling policy and calculates the scaling adjustment based on the metric and the target value. The scaling policy adds or removes capacity as required to keep the metric at, or close to, the specified target value.

- Deleting a customer master key (CMK) in AWS Key Management Service (AWS KMS) is destructive and potentially dangerous. Therefore, AWS KMS enforces a waiting period. To delete a CMK in AWS KMS you schedule key deletion. You can set the waiting period from a minimum of 7 days up to a maximum of 30 days. The default waiting period is 30 days. During the waiting period, the CMK status and key state is Pending deletion. To recover the CMK, you can cancel key deletion before the waiting period ends. After the waiting period ends you cannot cancel key deletion, and AWS KMS deletes the CMK.

- your application can achieve at least 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per prefix in a bucket.There are no limits to the number of prefixes in a bucket. You can increase your read or write performance by parallelizing reads. EFS is a costlier storage option compared to S3. There are no S3 data transfer charges when data is transferred in from the internet. Also with S3TA, you pay only for transfers that are accelerated

- Upgrades to the database engine level require downtime. Even if your RDS DB instance uses a Multi-AZ deployment, both the primary and standby DB instances are upgraded at the same time. This causes downtime until the upgrade is complete, and the duration of the downtime varies based on the size of your DB instance.

- If the Auto Scaling group (ASG) is using EC2 as the health check type and the Application Load Balancer (ALB) is using its in-built health check, there may be a situation where the ALB health check fails because the health check pings fail to receive a response from the instance. At the same time, ASG health check can come back as successful because it is based on EC2 based health check. Therefore, in this scenario, the ALB will remove the instance from its inventory, however, the ASG will fail to provide the replacement instance. This can lead to the scaling issues mentioned in the problem statement

- FSx for Windows does not allow you to present S3 objects as files and does not allow you to write changed data back to S3.

- kinesis throughput is needed to be manually provisioned using shards, unlike sqs which scales by itself

- Cost of test file storage on S3 Standard < Cost of test file storage on EFS < Cost of test file storage on EBS. EFS we pay for what we use, while ebs we pay for provisioned.

- S3 Intelligent-Tiering, S3 Standard-IA, and S3 One Zone-IA have a minimum storage duration charge of 30 days (so instead of 24 hours, you end up paying for 30 days even if u store only for 24 hrs). S3 Standard-IA and S3 One Zone-IA also have retrieval charges

- API Gateway supports stateless RESTful APIs as well as stateful WebSocket APIs

- When the new AMI is copied from region A into region B, it automatically creates a snapshot in region B because AMIs are based on the underlying snapshots. 

- ECS with EC2 launch type is charged based on EC2 instances and EBS volumes used. ECS with Fargate launch type is charged based on vCPU and memory resources that the containerized application requests

- When rebalancing, Amazon EC2 Auto Scaling launches new instances before terminating the old ones, so that rebalancing does not compromise the performance or availability of your application.However, the scaling activity of Auto Scaling works in a different sequence compared to the rebalancing activity. Auto Scaling creates a new scaling activity for terminating the unhealthy instance and then terminates it. Later, another scaling activity launches a new instance to replace the terminated instance.

- Amazon EFS is a regional service storing data within and across multiple Availability Zones (AZs) for high availability and durability. Amazon EC2 instances can access your file system across AZs, regions, and VPCs, while on-premises servers can access using AWS Direct Connect or AWS VPN. You can connect to Amazon EFS file systems from EC2 instances in other AWS regions using an inter-region VPC peering connection, and from on-premises servers using an AWS VPN connection

- A scheduled action sets the minimum, maximum, and desired sizes to what is specified by the scheduled action at the time specified by the scheduled action. For the given use case, the correct solution is to set the desired capacity to 10 as we need exact 10. When we want to specify a range of instances, then we must use min and max values. 

- in CF, Dynamic content, as determined at request time (cache-behavior configured to forward all headers), does not flow through regional edge caches, but goes directly to the origin. Proxy methods PUT/POST/PATCH/OPTIONS/DELETE go directly to the origin from the POPs and do not proxy through the regional edge caches

- A transit gateway is a network transit hub that you can use to interconnect your virtual private clouds (VPC) and on-premises networks.With AWS Direct Connect plus VPN, you can combine one or more AWS Direct Connect dedicated network connections with the Amazon VPC VPN. This combination provides an IPsec-encrypted private connection that also reduces network costs, increases bandwidth throughput, and provides a more consistent network experience than internet-based VPN connections. WS Direct Connect by itself cannot provide an encrypted connection between a data center and AWS Cloud,

- AWS Glue job is meant to be used for batch ETL data processing and it's not the right fit for a near real-time data processing use-case.

- Auto Scaling group lifecycle hooks enable you to perform custom actions as the Auto Scaling group launches or terminates instances. Lifecycle hooks enable you to perform custom actions by pausing instances as an Auto Scaling group launches or terminates them. When an instance is paused, it remains in a wait state either until you complete the lifecycle action using the complete-lifecycle-action command or the CompleteLifecycleAction operation, or until the timeout period ends

- Snowball Edge Storage Optimized is the optimal choice if you need to securely and quickly transfer dozens of terabytes to petabytes of data to AWS. It provides up to 80 TB of usable HDD storage, 40 vCPUs, 1 TB of SATA SSD storage, and up to 40 Gb network connectivity to address large scale data transfer and pre-processing use cases. Each Snowmobile has a total capacity of up to 100 petabytes. To migrate large datasets of 10PB or more in a single location, you should use Snowmobile.Direct Connect involves significant monetary investment and takes several months to set up,faster would be site to site vpn.

- For a small monthly monitoring and automation fee per object, Amazon S3 monitors access patterns of the objects in S3 Intelligent-Tiering and moves the ones that have not been accessed for 30 consecutive days to the infrequent access tier. If an object in the infrequent access tier is accessed, it is automatically moved back to the frequent access tier. .

- For Amazon Aurora, each Read Replica is associated with a priority tier (0-15). In the event of a failover, Amazon Aurora will promote the Read Replica that has the highest priority (the lowest numbered tier). If two or more Aurora Replicas share the same priority, then Amazon RDS promotes the replica that is largest in size. If two or more Aurora Replicas share the same priority and size, then Amazon Aurora promotes an arbitrary replica in the same promotion tier.

=================================================================================

-  AWS PrivateLink provides private connectivity between VPCs, AWS services, and on-premises applications, securely on the Amazon network. Withih a vpc, we can replicate between ec2 instances using private ip thereby avoiding data over internet and no need for private link.

- With launch templates, you can provision capacity across multiple instance types using both On-Demand Instances and Spot Instances to achieve the desired scale, performance, and cost.You cannot use a launch configuration to provision capacity across multiple instance types using both On-Demand Instances and Spot Instances

- You cannot use geolocation routing to reduce latency, hence this option is incorrect.As latency might be lower in some distant region itself. multi az is for availability and read replicas is for performance.

- AWS supports permissions boundaries for IAM entities (users or roles). A permissions boundary is an advanced feature for using a managed policy to set the maximum permissions that an identity-based policy can grant to an IAM entity.Service control policies (SCPs) are one type of policy that you can use to manage your organization.SCPs are available only in an organization that has all features enabled. SCPs aren't available if your organization has enabled only the consolidated billing features. So can use SCP only when organization is mentioned.

- Spot Instances with a defined duration (also known as Spot blocks) are designed not to be interrupted and will run continuously for the duration you select

- Amazon Kinesis Data Firehose  is a fully managed service that automatically scales to match the throughput of your data and requires no ongoing administration.Amazon Kinesis Data Streams, you can scale up to a sufficient number of shards (note, however, that you'll need to provision enough shards ahead of time). As it requires manual administration of shards.

- Amazon EFS Infrequent Access (EFS IA) is a storage class that provides price/performance that is cost-optimized for files, not accessed every day, with storage prices up to 92% lower compared to Amazon EFS Standard.

- AWS WAF is a web application firewall that helps protect your web applications or APIs against common web exploits  such as SQL injection or cross-site scripting, and rules that filter out specific traffic patterns you define.A web access control list (web ACL) gives you fine-grained control over the web requests that your Amazon CloudFront distribution, Amazon API Gateway API, or Application Load Balancer responds to.If you want to allow or block web requests based on the IP addresses that the requests originate from, create one or more IP match conditions.
AWS Shield Advanced will give you DDoS protection overall, and you cannot set up rate-based rules in Shield. WAF allows rate based rules

- The Spot Fleet selects the Spot Instance pools that meet your needs and launches Spot Instances to meet the target capacity for the fleet. By default, Spot Fleets are set to maintain target capacity by launching replacement instances after Spot Instances in the fleet are terminated.. In this case, we want to use the most cost-optimal option and leave the selection of the cheapest spot instance to a Spot Fleet request, which can be optimized with the lowestPrice strategy.  Spot Fleet requests will help launch a mix of On-Demand and Spot, but won't have the auto-scaling capability we need

- When a Kinesis data stream is configured as the source of a Firehose delivery stream, Firehose’s PutRecord and PutRecordBatch operations are disabled and Kinesis Agent cannot write to Firehose delivery stream directly. Data needs to be added to the Kinesis data stream through the Kinesis Data Streams PutRecord and PutRecords operations instead. 

- S3 Glacier Deep Archive is up to 75% less expensive than S3 Glacier and provides retrieval within 12 hours.

- AWS Transit Gateway is a service that enables customers to connect their Amazon Virtual Private Clouds (VPCs) and their on-premises networks to a single gateway. With AWS Transit Gateway, you only have to create and manage a single connection from the central gateway into each Amazon VPC, on-premises data center, or remote office across your network. Transit Gateway acts as a hub that controls how traffic is routed among all the connected networks which act like spokes. 

- Network Load Balancer or Classic Load Balancer cannot be used to route traffic based on the content of the request. Only ALB can do that.

- Aurora has global aurora and dynamo has global tables

- The following are the default rules for a default security group: Allow inbound traffic from network interfaces (and their associated instances) that are assigned to the same security group. Allows all outbound traffic.The following are the default rules for a security group that you create: Allows no inbound traffic Allows all outbound traffic

- If the master database is not encrypted, the read replicas cannot be encrypted. If the master database is encrypted, the read replicas are necessarily encrypted. You can only enable encryption for an Amazon RDS DB instance when you create it, not after the DB instance is created.However, because you can encrypt a copy of an unencrypted DB snapshot, you can effectively add encryption to an unencrypted DB instance. That is, you can create a snapshot of your DB instance, and then create an encrypted copy of that snapshot. 

- partition placement is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka. Spread is not suitable for this.

- To migrate accounts from one organization to another, you must have root or IAM access to both the member and master accounts. Here are the steps to follow: 1. Remove the member account from the old organization 2. Send an invite to the new organization 3. Accept the invite to the new organization from the member account

- Elastic Load Balancing stops sending requests to targets that are deregistering. By default, Elastic Load Balancing waits 300 seconds before completing the deregistration process, which can help in-flight requests to the target to complete. You cannot use sticky sessions to stop the current request from being interrupted in case of a scale-in event.

- AWS Resource Access Manager (RAM) is a service that enables you to easily and securely share AWS resources with any AWS account or within your AWS Organization. You can share AWS Transit Gateways, Subnets, AWS License Manager configurations, and Amazon Route 53 Resolver rules resources with RAM. So we can create on vpc and share subnets across accounts. VPC peering connections will work, but won't efficiently scale if you add more accounts as we need to make more connections.A Transit Gateway can also enable inter vpc communication, but will be an expensive solution. 

- Bastion Hosts are using the SSH protocol, which is a TCP based protocol on port 22. They must be publicly accessible. Network Load Balancer, which supports TCP traffic, and will automatically allow you to connect to the EC2 instance in the backend.

- Read replicas are meant to address scalability issues. You cannot use read replicas for improving availability

- bucket-name.s3-website.Region.amazonaws.com 

- all S3 GET, PUT, and LIST operations, as well as operations that change object tags, ACLs, or metadata, are strongly consistent. What you write is what you will read, and the results of a LIST will be an accurate reflection of what’s in the bucket.Amazon S3 delivers strong read-after-write consistency automatically. This is from 2020 aws. Before u might get old data until write is complete. So exam before jun 21 this is the correct way

- Listbucket is applied at bucket level so rule ends with / and not /* (which is for objects)

- A byte-range request is a perfect way to get the beginning of a file and ensuring we remain efficient during our scan of our S3 bucket.You cannot use Byte Range Fetch parameter with S3 Select. Please note that with Amazon S3 Select, you can scan a subset of an object by specifying a range of bytes to query using the ScanRange parameter. This capability lets you parallelize scanning the whole object by splitting the work into separate Amazon S3 Select requests for a series of non-overlapping scan ranges. Use the Amazon S3 Select ScanRange parameter and Start at (Byte) and End at (Byte).

- Currently, the Standard SQS queue is only allowed as an Amazon S3 event notification destination, whereas the FIFO SQS queue is not allowed.

- Elasticache for Memcached is not HIPAA eligible

- we need to launch the Read Replica in the same AZ, because we have to pay for inter-AZ data transfer, whereas the transfer of data within a single AZ is free.so read replica in same az is cheaper
============================+===+=+
-  If the spot request is persistent, the request is opened again after your Spot Instance is interrupted. If the request is persistent and you stop your Spot Instance, the request only opens after you start your Spot Instance. Spot Instances with a defined duration (also known as Spot blocks) are designed not to be interrupted and will run continuously for the duration you select. You can use a duration of 1, 2, 3, 4, 5, or 6 hours. In rare situations, Spot blocks may be interrupted due to Amazon EC2 capacity needs

- Think resource performance monitoring, events, and alerts; think CloudWatch.Think account-specific activity and audit; think CloudTrail.Think resource-specific history, audit, and compliance; think Config

- If you have multiple AWS Site-to-Site VPN connections, you can provide secure communication between sites using the AWS VPN CloudHub. This enables your remote sites to communicate with each other, and not just with the VPC. Sites that use AWS Direct Connect connections to the virtual private gateway can also be part of the AWS VPN CloudHub. The VPN CloudHub operates on a simple hub-and-spoke model that you can use with or without a VPC. This design is suitable if you have multiple branch offices and existing internet connections and would like to implement a convenient, potentially low-cost hub-and-spoke model for primary or backup connectivity between these remote offices

- With cached volumes, the AWS Volume Gateway stores the full volume in its Amazon S3 service bucket, and just the recently accessed data is retained in the gateway’s local cache for low-latency access. With stored volumes, your entire data volume is available locally in the gateway, for fast read access. Volume Gateway also maintains an asynchronous copy of your stored volume in the service’s Amazon S3 bucket

- You can use message timers to set an initial invisibility period for a message added to a queue. So, if you send a message with a 60-second timer, the message isn't visible to consumers for its first 60 seconds in the queue. The default (minimum) delay for a message is 0 seconds. The maximum is 15 minutes

- When cross-zone load balancing is enabled, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones. When cross-zone load balancing is disabled, each load balancer node distributes traffic only across the registered targets in its Availability Zone.so if 1 in AZ a and 4 in AZ b, with cross zone each get 20 while with cross zone disabled aza gets 50 and 4 in azb gets 50

- If a user or role has an IAM permission policy that grants access to an action that is either not allowed or explicitly denied by the applicable SCPs, the user or role can't perform that action.
SCPs affect all users and roles in the attached accounts, including the root user.
SCPs do not affect any service-linked role.