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

- With AWS Managed Microsoft AD, you can run directory-aware workloads in the AWS Cloud such as SQL Server-based applications. You can also configure a trust relationship between AWS Managed Microsoft AD in the AWS Cloud and your existing on-premises Microsoft Active Directory, providing users and groups with access to resources in either domain, using single sign-on (SSO).AD Connector simply connects your existing on-premises Active Directory to AWS. You cannot use it to run directory-aware workloads on AWS

- Just remember that you should use AD Connector if you only need to allow your on-premises users to log in to AWS applications with their Active Directory credentials. AWS Managed Microsoft AD would also allow you to run directory-aware workloads in the AWS Cloud. AWS Managed Microsoft AD is your best choice if you have more than 5,000 users and need a trust relationship set up between an AWS hosted directory and your on-premises directories. Simple AD is the least expensive option and your best choice if you have 5,000 or fewer users and don’t need the more advanced Microsoft Active Directory features such as trust relationships with other domains.

- An Internet Gateway serves two purposes: to provide a target in your VPC route tables for internet-routable traffic and to perform network address translation (NAT) for instances that have been assigned public IPv4 addresses

- You can copy both Amazon EBS-backed AMIs and instance-store-backed AMIs. You can share an AMI with another AWS account. To copy an AMI that was shared with you from another account, the owner of the source AMI must grant you read permissions for the storage that backs the AMI, either the associated EBS snapshot (for an Amazon EBS-backed AMI) or an associated S3 bucket (for an instance store-backed AMI).

- You can't convert an existing standard queue into a FIFO queue. To make the move, you must either create a new FIFO queue for your application or delete your existing standard queue and recreate it as a FIFO queue.

- Dedicated Hosts enable you to use your existing server-bound software licenses like Windows Server and address corporate compliance and regulatory requirements. Dedicated instances cannot be used for existing server-bound software licenses.

- AWS Global Accelerator is a networking service that helps you improve the availability and performance of the applications that you offer to your global users. AWS Global Accelerator is easy to set up, configure, and manage. It provides static IP addresses that provide a fixed entry point to your applications and eliminate the complexity of managing specific IP addresses for different AWS Regions and Availability Zones. AWS Global Accelerator always routes user traffic to the optimal endpoint based on performance, reacting instantly to changes in application health, your user’s location, and policies that you configure. Global Accelerator is a good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP.CloudFront supports HTTP/RTMP protocol based requests. Global Accelerator is a good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP, as well as for HTTP use cases that specifically require static IP addresses or deterministic, fast regional failover. Both services integrate with AWS Shield for DDoS protection

- Metadata, which can be included with the object, is not encrypted while being stored on Amazon S3.

- Terminated instances cannot be recovered. A recovered instance is identical to the original instance, including the instance ID, private IP addresses, Elastic IP addresses, and all instance metadata. If the impaired instance is in a placement group, the recovered instance runs in the placement group. If your instance has a public IPv4 address, it retains the public IPv4 address after recovery. During instance recovery, the instance is migrated during an instance reboot, and any data that is in-memory is lost.

- Amazon VPC provides the facility to create an IPsec VPN connection (also known as site-to-site VPN) between remote customer networks and their Amazon VPC over the internet. Virtual private gateway: A Virtual Private Gateway (also known as a VPN Gateway) is the endpoint on the AWS VPC side of your VPN connection. Customer Gateway: An AWS resource that provides information to AWS about your Customer Gateway device.Customer Gateway device: A physical device or software application on the customer side of the Site-to-Site VPN connection.

- Amazon Kinesis Data Streams is recommended when you need the ability for multiple applications to consume the same stream concurrently compared to sqs

- You can change the tenancy of an instance from dedicated to host. You can change the tenancy of an instance from host to dedicated

- we can create VPC with a single public subnet , public and private, public private and site to site VPN, private and site to site VPN. Public and site to site VPN is not allowed 

- AWS CloudFormation StackSet extends the functionality of stacks by enabling you to create, update, or delete stacks across multiple accounts and regions with a single operation. A stack set lets you create stacks in AWS accounts across regions by using a single AWS CloudFormation template. Using an administrator account of an "AWS Organization", you define and manage an AWS CloudFormation template, and use the template as the basis for provisioning stacks into selected target accounts of an "AWS Organization" across specified regions.

- NAT instance can be used as a bastion server. Security Groups can be associated with a NAT instance. NAT instance supports port forwarding

- The main issue with simple scaling is that after a scaling activity is started, the policy must wait for the scaling activity or health check replacement to complete and the cooldown period to expire before responding to additional alarms. And step scaling is not as efficient as the target tracking policy where you can calculate the exact number of instances required to handle the spike in orders

- By default, basic monitoring is enabled when you use the AWS management console to create a launch configuration. Detailed monitoring is enabled by default when you create a launch configuration using the AWS CLI

- When you create a launch configuration, the default value for the instance placement tenancy is null and the instance tenancy is controlled by the tenancy attribute of the VPC. If you set the Launch Configuration Tenancy to default and the VPC Tenancy is set to dedicated, then the instances have dedicated tenancy. If you set the Launch Configuration Tenancy to dedicated and the VPC Tenancy is set to default, then again the instances have dedicated tenancy.

- You should also note that Route 53 doesn't charge for alias queries to AWS resources but Route 53 does charge for CNAME queries. Additionally, an alias record can only redirect queries to selected AWS resources such as S3 buckets, CloudFront distributions, and another record in the same Route 53 hosted zone; however a CNAME record can redirect DNS queries to any DNS record. So, you can create a CNAME record that redirects queries from app.covid19survey.com to app.covid19survey.net

- To coordinate Availability Zones across accounts, you must use the AZ ID, which is a unique and consistent identifier for an Availability Zone. For example, usw2-az2 is an AZ ID for the us-west-2 region and it has the same location in every AWS account

- AWS Step Functions lets you coordinate and orchestrate multiple AWS services such as AWS Lambda and AWS Glue into serverless workflows

- Delay queues let you postpone the delivery of all new messages to a queue for several SECONDS.You can use message timers to set an initial invisibility period for a message added to a queue

- With AWS Database Migration Service, you can continuously replicate your data with high availability and consolidate databases into a petabyte-scale data warehouse by streaming data to Amazon Redshift and Amazon S3. The Amazon Redshift cluster must be in the same AWS account and the same AWS Region as the replication instance. During a database migration to Amazon Redshift, AWS DMS first moves data to an Amazon S3 bucket.

- nlb. If you specify targets using an instance ID, traffic is routed to instances using the primary private IP address specified in the primary network interface for the instance. If you specify targets using IP addresses, you can route traffic to an instance using any private IP address from one or more network interfaces. This enables multiple applications on an instance to use the same port

- In Kinesis Data Streams,  You should use enhanced fan-out if you have multiple consumers retrieving data from a stream in parallel. With enhanced fan-out developers can register stream consumers to use enhanced fan-out and receive their own 2MB/second pipe of read throughput per shard, and this throughput automatically scales with the number of shards in a stream

- Data sync is natively integrated with Amazon S3, Amazon EFS, Amazon FSx for Windows File Server, Amazon CloudWatch, and AWS CloudTrail

- Therefore, you cannot use an Internet Gateway ID as the custom source for the inbound rule.

========++==++============

- Route53 Simple Records do not have health checks,

- Redis authentication tokens enable Redis to require a token (password) before allowing clients to execute commands, thereby improving data security. IAM Auth is not supported by ElastiCache

- S3 Identity-based policies – Attach managed and inline policies to IAM identities (users, groups to which users belong, or roles). Identity-based policies grant permissions to an identity.Resource-based policies – Attach inline policies to resources. The most common examples of resource-based policies are Amazon S3 bucket policies and IAM role trust policies. Resource-based policies grant permissions to the principal that is specified in the policy. Principals can be in the same account as the resource or in other accounts.

- After you change the launch configuration for an Auto Scaling group, any new instances are launched using the new configuration options, but existing instances are not affected. To update the existing instances, terminate them so that they are replaced by your Auto Scaling group, or allow automatic scaling to gradually replace older instances with newer instances based on your termination policies

- The following are the characteristics of security group rules: 1. By default, security groups allow all outbound traffic. 2. Security group rules are always permissive; you can't create rules that deny access. 3. Security groups are stateful

- Redshift's internal storage does not have "tiers" of storage classes like Amazon S3,Moving the data to S3 glacier will prevent Athena from being able to query it.

- You configure the size of your Auto Scaling group by setting the minimum, maximum, and desired capacity. The minimum and maximum capacity are required to create an Auto Scaling group, while the desired capacity is optional. If you do not define your desired capacity upfront, it defaults to your minimum capacity. An Auto Scaling group is elastic as long as it has different values for minimum and maximum capacity. All requests to change the Auto Scaling group's desired capacity (either by manual scaling or automatic scaling) must fall within these limits.

- Use IAM authentication from Lambda to RDS Postgresql. Attach an AWS Identity and Access Management (IAM) role to AWS Lambda. You can authenticate to your DB instance using AWS Identity and Access Management (IAM) database authentication. IAM database authentication works with MySQL and PostgreSQL

- there are certain limits you should keep in mind while using Amazon Kinesis Data Streams:

By default, records of a stream are accessible for up to 24 hours from the time they are added to the stream. You can raise this limit to up to 7 days by enabling extended data retention.The maximum size of a data blob (the data payload before Base64-encoding) within one record is 1 megabyte (MB). Each shard can support up to 1000 PUT records per second. The throughput of an Amazon Kinesis data stream is designed to scale without limits via increasing the number of shards within a data stream.Kinesis cannot scale infinitely

- Elastic Beanstalk cannot manage AWS Lambda functions

- You can enable API caching in Amazon API Gateway to cache your endpoint's responses. With caching, you can reduce the number of calls made to your endpoint and also improve the latency of requests to your API. When you enable caching for a stage, API Gateway caches responses from your endpoint for a specified time-to-live (TTL) period, in seconds.

- Aurora Read Replicas are independent endpoints in an Aurora DB cluster, best used for scaling read operations and increasing availability.

- Network Load Balancers expose a fixed IP to the public web, therefore allowing your application to be predictably reached using these IPs, while allowing you to scale your application behind the Network Load Balancer using an ASG. Application and Classic Load Balancers expose a fixed DNS (=URL) rather than the IP address.

- Dynamic port mapping with an Application Load Balancer makes it easier to run multiple tasks on the same Amazon ECS service on an Amazon ECS cluster.

- To control whether an Auto Scaling group can terminate a particular instance when scaling in, use instance scale-in protection. You can enable the instance scale-in protection setting on an Auto Scaling group or on an individual Auto Scaling instance. When the Auto Scaling group launches an instance, it inherits the instance scale-in protection setting of the Auto Scaling group. You can change the instance scale-in protection setting for an Auto Scaling group or an Auto Scaling instance at any time

=================+==++=+=====

- The CloudWatch recovery option works only for system check failures, not for instance status check failures. Also, if you terminate your instance, then it can't be recovered.The automatic recovery process attempts to recover your instance for up to three separate failures per day. Your instance may subsequently be retired if automatic recovery fails and a hardware degradation is determined to be the root cause for the original system status check failure. 

- Access advisor will determine the permissions your developers have used by analyzing the last timestamp when an IAM entity (for example, a user, role, or group) accessed an AWS service. This information helps you audit service access, remove unnecessary permissions, and set appropriate permissions across different environments 

- Since multiple applications need to consume the same data stream concurrently, Kinesis is a better choice when compared to the combination of SQS with SNS. Kinesis is a publish-subscribe model, used when publisher applications need to publish the same data to different consumers in parallel. When a host needs to send many records per second (RPS) to Amazon Kinesis, simply calling the basic PutRecord API action in a loop is inadequate. To reduce overhead and increase throughput, the application must batch records and implement parallel HTTP requests.

- CloudFront works from edge locations and doesn't belong to a VPC.You can use AWS WAF with your Application Load Balancer to allow or block requests based on the rules in a web access control list (web ACL) and so works with vpc

- By default, all DynamoDB tables are encrypted under an AWS owned customer master key (CMK), which do not write to CloudTrail logs. You do not need to create or manage the AWS owned CMKs. However, you cannot view, use, track, or audit them. You are not charged a monthly fee or usage fee for AWS owned CMKs and they do not count against the AWS KMS quotas for your account.The key rotation strategy for an AWS owned CMK is determined by the AWS service that creates and manages the CMK. All DynamoDB tables are encrypted. There is no option to enable or disable encryption for new or existing tables. By default, all tables are encrypted under an AWS owned customer master key (CMK) in the DynamoDB service account. However, you can select an option to encrypt some or all of your tables under a customer-managed CMK or the AWS managed CMK for DynamoDB in your account.

- securely authenticate users for accessing your applications. This enables you to offload the work of authenticating users to your load balancer so that your applications can focus on their business logic. You can use Cognito User Pools to authenticate users . Amazon Cognito identity pools provide temporary AWS credentials for users who are guests (unauthenticated) and for users who have been authenticated and received a token. You cannot directly integrate Cognito User Pools with CloudFront distribution as you have to create a separate Lambda@Edge function to accomplish the authentication via Cognito User Pools.

-When these situations occur, Amazon EC2 Auto Scaling chooses the policy that provides the largest capacity for both scale-out and scale-in

- Since Lambda functions can scale extremely quickly, this means you should have controls in place to notify you when you have a spike in concurrency. Cold start has no dependancy on lambda size 

- With Global Accelerator, you are provided two global static customer-facing IPs to simplify traffic management. On the back end, add or remove your AWS application origins, such as Network Load Balancers, Application Load Balancers, Elastic IPs, and EC2 Instances, without making user-facing changes. To mitigate endpoint failure, Global Accelerator automatically re-routes your traffic to your nearest healthy available endpoint

- Amazon EC2 Auto Scaling doesn't terminate an instance that came into service based on EC2 status checks and ELB health checks until the health check grace period expires.Amazon EC2 Auto Scaling does not immediately terminate instances with an Impaired status. Instead, Amazon EC2 Auto Scaling waits a few minutes for the instance to recover. Amazon EC2 Auto Scaling might also delay or not terminate instances that fail to report data for status checks. This usually happens when there is insufficient data for the status check metrics in Amazon CloudWatch. By default, Amazon EC2 Auto Scaling doesn't use the results of ELB health checks to determine an instance's health status when the group's health check configuration is set to EC2. As a result, Amazon EC2 Auto Scaling doesn't terminate instances that fail ELB health checks.Amazon EC2 Auto Scaling terminates Spot instances when capacity is no longer available or the Spot price exceeds your maximum price. 

-
AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture. With X-Ray, you can understand how your application and its underlying services are performing to identify and troubleshoot the root cause of performance issues and errors. You can use X-Ray to collect data across AWS Accounts. The X-Ray agent can assume a role to publish data into an account different from the one in which it is running. This enables you to publish data from various components of your application into a central account. You cannot use CloudTrail to debug and trace data across accounts

- You must ensure that your load balancer can communicate with registered targets on both the listener port and the health check port. you must verify that the security groups associated with the load balancer allow traffic on the new port in both directions.

- When you create an EBS volume, it is automatically replicated within its Availability Zone to prevent data loss due to the failure of any single hardware component. You can attach an EBS volume to an EC2 instance in the same Availability Zone

-With IAM policies, you can only grant users within your own AWS account permission to access your Amazon S3 resources. With bucket policies, you can grant users within your AWS Account or other AWS Accounts access to your Amazon S3 resources. With ACLs, you can only grant other AWS accounts (not specific users) access to your Amazon S3 resources

- By combining the results of health checks of your EC2 instances and your ELBs, Route 53 DNS Failover can evaluate the health of the load balancer and the health of the application running on the EC2 instances behind it. In other words, if any part of the stack goes down, Route 53 detects the failure and routes traffic away from the failed endpoint.

- Unlike Global Accelerator, managing and routing to different instances, ELBs and other AWS resources will become an operational overhead as the resource count reaches into the hundreds. With inbuilt features like Static anycast IP addresses, fault tolerance using network zones, Global performance-based routing, TCP Termination at the Edge, Global Accelerator is the right choice for multi-region, low latency use cases

- Dedicated Instances are Amazon EC2 instances that run in a virtual private cloud (VPC) on hardware that's dedicated to a single customer. Dedicated Instances that belong to different AWS accounts are physically isolated at a hardware level, even if those accounts are linked to a single-payer account. However, Dedicated Instances may share hardware with other instances from the same AWS account that are not Dedicated Instances.A Dedicated Host is also a physical server that's dedicated for your use. With a Dedicated Host, you have visibility and control over how instances are placed on the serve

-Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts, workloads, and data stored in Amazon S3. GuardDuty analyzes tens of billions of events across multiple AWS data sources, such as AWS CloudTrail events, Amazon VPC Flow Logs, and DNS logs. Disabling the service will delete all remaining data, including your findings and configurations before relinquishing the service permissions and resetting the service

-While relying on the DNS service is a great option for blue/green deployments, it may not fit use-cases that require a fast and controlled transition of the traffic. Some client devices and internet resolvers cache DNS answers for long periods. With AWS Global Accelerator, you can shift traffic gradually or all at once between the blue and the green environment and vice-versa without being subject to DNS caching

-  If you have objects that are smaller than 1GB or if the data set is less than 1GB in size, you should consider using Amazon CloudFront's PUT/POST commands for optimal performance. The given use case has data larger than 1GB and hence S3 Transfer Acceleration is a better option

- If you have an EC2 Auto Scaling group (ASG) with running instances and you choose to delete the ASG, the instances will be terminated and the ASG will be deleted. Data is not automatically copied from existing instances to new instances. You can use lifecycle hooks to copy the data. EC2 Auto Scaling groups are regional constructs. They can span Availability Zones, but not AWS Regions. Amazon EC2 Auto Scaling doesn't automatically add a volume when the existing one is approaching capacity. 

-Amazon EventBridge is recommended when you want to build an application that reacts to events from SaaS applications and/or AWS services. Amazon EventBridge is the only event-based service that integrates directly with third-party SaaS partners. Amazon EventBridge also automatically ingests events from over 90 AWS services without requiring developers to create any resources in their account.

- When Lambda is directly configured with SES, Amazon SES sends an event record to Lambda every time it receives an incoming message. However, it omits the body of the message. We will need the entire message for the current use case.

-Also known as dogpiling, the thundering herd effect is what happens when many different application processes simultaneously request a cache key, get a cache miss, and then each hits the same database query in parallel. The more expensive this query is, the bigger impact it has on the database.One problem with adding TTLs to all of your cache keys is that it can exacerbate this problem. For example, let's say millions of people are following a popular user on your site. That user hasn't updated his profile or published any new messages, yet his profile cache still expires due to a TTL. Your database might suddenly be swamped with a series of identical queries.The solution is to prewarm the cache. The write-through strategy adds data or updates data in the cache whenever data is written to the database. Every write involves two trips: A write to the database and then a write to the cache, which adds latency to the process

- By default, an S3 object is owned by the AWS account that uploaded it. This is true even when the bucket is owned by another account. Because the Amazon Redshift data files from the UNLOAD command were put into your bucket by another account, you (the bucket owner) don't have default access. 

- To scale read capacity, ElastiCache allows you to add up to five read replicas across multiple availability zones

- AWS WAF is tightly integrated with Amazon CloudFront and the Application Load Balancer (ALB), services that AWS customers commonly use to deliver content for their websites and applications. When you use AWS WAF on Amazon CloudFront, your rules run in all AWS Edge Locations,

- Amazon Elastic File System (Amazon EFS) is regionally scoped storage service. Amazon EFS allows you to mount a single volume to multiple instances, and is stored redundantly across multiple AZs within that single region. 

- AWS in its Reliability pillar states that the load on systems shouldn't drastically change from a normal working day to a heavy load day.

- AWS Cost Explorer helps you identify under-utilized EC2 instances that may be downsized on an instance by instance basis within the same instance family, and also understand the potential impact on your AWS bill by taking into account your Reserved Instances and Savings Plans.

- AWS DMS supports specifying Amazon S3 as the source and streaming services like Kinesis and Amazon Managed Streaming of Kafka (Amazon MSK) as the target.
