
- IAM Its Global
- Cloudwatch alerts can be on static data or anomaly detection
- CW alerts needs email to be confirmed? or is for SNS?

### S3 
> Check Differetn s3 solutins and their coast

- S3 is object storage. Its universal name spaced.
- **S3 File size is from 0 to 5TB**
- S3 usually has >=3 availabilty zones
- S3 site is like s3-region.amazonaws.com/bucketname
- S3 files consists of key(name of file) value (data) , version, metadata, subresoources (access control list, bucket policies etc), torrent etc.
- **Read after Write for put new objects. Eventual consistency for overwrite put and delete**
- 99.99% availability. 99 and 11 9's durability ( stored redudantan and designed to withstand loss of 2 az concurrently)
- S3 has tiered storage and lifecycle storage, versioned storage, ecnrypted storage, MFA delete,
- S3 standard is what is common. S3-IA( infrequent accessed, 99,99). Can be retrieved fast when needed, but charged for retreival. Also has S3 One Zone- IA ( only one availablity zone, lowest cost, 99.5). S3 Intelligent Tiering ( amazon moves object based on how we access the objects)
-Glacier : S3 Glacier Standard  (retireval of mins to few hours, store any amount of data), S3 Glacier Deep archive ( retrieve time of 12 hours is acceptable lowest cost)
- No retrieve cost for standard S3 and INtelligent S3
-**S3 glaciers need min 40kb object size,  S3 standard-IA and one zone -IA needs 128kb min object size**
- S3 has cross region replication to replicate data. Also has transfer accelaration using cloudfront.
- S3 bucket can have access log that can be send to another bucket or bucekt in another account
- S3 encryption at rest can be server side or client side. Server side encryption can be with s3 managed keys (sse-s3), aws key management service managed keys (kms) and customer provided keys
- With versioning s3 stores all variations of object, even deleted ones. so it s a backup tool as well
- **S3 versioning once turned on cannot be turned off only suspended**
- Versioning also comes with MFA delete protection
- **When we upload a new version of file, it doesnt get permissions and security settings of old file. It goes to bucket default.  But old ones will retain its settings.**
- All versions are stored in bucket. **By default only latest is shown. But all versions takes up space.**
- On deleting a file in versioned s3, file is retained but a new link with same name as file and a delete marker is created. To restore file, delete this link with delete marker set.
- **If we delete versions directly, we cannot recover them**
- We can have life cycle rules for current version or previous version or both.
- We can set rule like move to glacier after 60 days or so.
- **S3 cross region replication requires versioning enabled**
- **When we enable cross region replication on an existing bucket, the existing objects are not copied, not even the versions.We need to mannually replciate them.**
- **If we delete a file, its not deleted in the replicated folder, even if we delete the version.**
- There is webpage to upload using transfer accelaraion ( using edge locations). 

### CloudFront
- edge location is files will be cached .Origin is where files comes frmo like s3 or ec2 etc. 
- **A distributin is a collection of edge locations for a cdn**
- Edge locations can be written to as well. We can set ttl We can clear cached objects but will be charged.
- **Cloudfront is global**
- There are two types of distributios: Web and RTMP (media)

### Snowball
- Used to transfer large data in disks. **Either 50Tb or 80tb**. secured by 256bit encryption, tamper proof etc
- snowball can import or export to s3.
- **Snowball edge is 100TB device ** with storage and compute capabilities.
- Snow mobile is a exabyte data transfer data.  its a huge shipping container

### Storage Gateway
- it a service that connects on premise software appliance to cloud 
- its available for download as a virtual machine
- Three types of storage gateways:**File gateway (NFS, SMB) and Volume Gateway(iSCSI)**, which has two types: Stored volumes and Cached Volumes
- Third is volume gateway - Tape gateway (Virtual Tage interface)
- For File storage, files are stored in s3 and can be accessed via an NFS mount point in storage gateway
- Volume gateway is for entire disk storage. Supports async backup and incremental backup. Backup happends to s3

### Ec2
- Types of ec2 available
- On demand. pay when you use
- **Reserved instances. Can be standard(70% less cost) or converable(50% less cost)**. Here pay less as you reserve more. We also has scheduled reserved instances.
- spot bidding instances price moves around depending on demand
- dedeicated ec2 machines. useful in regulatory use cases. can be purchased on demand or reserved.
- **termination protection is turned off by default**
- **on ebs backed instance, default action is for root volume to be deleted on termination**
- volume of default ami can be encrypted.this can be done while creating new instance or via thrid party tool. additional volumes can also be encrypteds

### Security Groups
- Every change is applied immediately
- **securirty rules are stateful. So every rule inbound allowed is auto allowed outbound. So deleting outbiudn rules but keeping inboud rule, will make inbound rules copy to outbound.  NACL is stateless though. **

- We cannot blacklist an ip with security group. We do reverse in SG, like allow something. By default all is blocked. But do same with NACL
- **We can attach more than one SG to an ec2**. SO effect will be sum of all SG

### EBS
- elastic block storage. **Each ebs is auto replicated within its availabilty zone**. 
- it has 5 types. General ssd, provisioned iops ssd, thoughput optimized (magnetic), cold (magnetic), and magnetic 
- provisioned iops for DB, thoughout for data warehouse, cold for file servers, magentic for infrequent access

### Volumes and Snapshots
- **volumes will be in same availability zone as ec2**
- Terminate an instance causes root ebs volume to be deleted as well, **but additional attached ebs volumes will persist.**
- **ebs volumes we can edit after attaching. Like change size or iops etc**. These can be done on the fly
- We can create a snapshot of a volume. And then create an image or volume from that snapshot .snapshots exist in s3. snapshots are incremental
- **to take snapshot of a root volume of a running instance, we first need to stop the instance. **
- We can copy an AMI from one region to another. Combined with above step we can move our data from one region to another.

### AMI
- you can select ami based on region, operating system, architecture, launch permission, root volume device(storage type)
- all amis are either instance store or ebs backed.
- **instance stored instances cannot be stopped. only terminate. Ebs backed can be stopped and data is persisted there. We can reboot both, and data wont be lost.**
- **Root volume is lost on terminatin for both, but we can tell aws to not delete root volume for ebs backed.**

### Encrypted Volumes
- earlier we used launch instance, take snapshot, take a copy and encrpt and then create image and lauch from that to get encrupted instance. Now we haave an option to encrypt during launch itself.

- if we launch an instance from encrypt volume, we have to choose option enctypted in storage selection. It wont allow us to proceed without that

### Cloudwatch
- to monitor aws resources and applications. It monitors performance. 
- in ec2 CW can monitor cpu, memory, disk , status. **it monitors every 5 mins by default for standard monitoring. detailed monitoring is 1 min**
- cloudtrail is used to track user activity.

### EC2 config and scripts
- we can attach a role to an existing ec2 role, using the command line or console.
- **Roles are universal and can be used any where**
- we can set up a script to run when booting an ec2 instance.
- 169.254.169.254/latest/user-data gives bootstrap scripts from within ec2
-169.254.169.254/latest/metadata/options gives metadata depending on option given frmo within ec2

### EFS
- efs is file storage service for ec2. can be shared with ec2 instances, can grow as needed
- efs ec2 commnunication is via nfs protocol? port 2049. Supports nfs v4
- **we only pay for storage taht we use. no pre provisining required.**
- **data stored within multiple az in a region and has read after write consistency.**
- after creating an efs, we log into ec2 and mount that efs. We can mount same efs to different ec2

### EC2 placement group
- has 3 types: clustered, spread and partitioned placement groups
- clustered, ec2 instances within an avaiality zone, for apps needing low network latency, high through put, and only few apps can be launched into that clustered zone.
- spread : instances placed on distinct hardwarde, separate racks, separate power etc. For critical instances. spread can be within same availanlity zone or different. specifically for individial insatnces in individual rack
- spread can have max 7 running instances in one group
- partition: same as spread, but isntances placed in partitions within an availablity zone. aws ensures that each partitioin within an availability zone has its own rack with its own network and power zone. use cases is cassandra clusters. **It cannot span multipe availabilty zone, spread can. Cluster can also be in one availability zone. but all has to be in same region.**
- **aws recommend homogenous instances within cluster**
- cant merge placement groups
- cant move instances within placement gruops, so take ami image and move.

### Database
- aws has oracle, msql, maria, aurora, postgre, sql server as rdbms
= rds(oltp) in aws has multi az for disaster recovery and read replicas for performance. multi az has auto fail over
- oltp is online transaction processing and olap is online analytics processing
- **aws dataware house is redshift**
- elastic cache is in memory cache with scalability (memcahced and redis)
- rds runs on virtual servers. but we cannot log into those servers, **hence its not serverless**
- rds has two backups, automated and snapshots. automated has a retention period of 1 to 35 days.its enabled by default, stored in s3. backup is taken in a daefult window and may experience a delay. backup is automated
- snapshot is done manually. **restorign from snapshot results in a new db, with new endpoint**
- encryption at rest is done using aws KMS service.its done for data, backup, read repicas, snapshots.
- aws aurora is by design fault tolerant. So multi az is available for all others and not needed for aurora
- **read replicas is available for all except mssql.**
- **read replicas need automated backup turned on, 5 read replicas of a db. also we can have read replicas of read replicas.**
- each read replica has its own end point, can be in multi az, we can created read replicas of multi az dbs. We can promote read replcas to main database.
- dynamodb supports both document and key value storage. its stored on ssd
- spread across 3 different geo. it has eventual consistent reads (default) adn strongly consistent reads, based on 1 sec rule. eventual consistent , data is consistent for read after 1 sec
- redshit is a fully managed peta byte fast data warehouse.
- red shit can be singe node(160gb) or multi node( has leader node and compute nodes). it also has massive parallel processing. can have backups, enabled by deafult. redshift always maintains 3 copies of data, can async repicate data to s3 in a diff region. billed on compute horus and backups and data transfer(vpc)
- redshit encrypt in transit, at rest: noetc. available only in one AZ no multi AZ.
- aurora is a mysql compatable rdbms db. 2 copies of data in each az, with min of 3 az, so 6 copies fo data in 3 az in total. can handle loss of nodes by itself.
- aurora can have its own replices and mysql read replcas. we can share aurora snapshot with other aws accounts
- **redis supports multi az**
- **if we run a db in ec2 aws recommends EBS as storage**
- for rds security group no port no is needed as its pickup automatically based on rds
- When you have deployed an RDS database into multiple availability zones, can you use the secondary database as an independent read node: no

### Route53
- every domain name record starts with SOA record( Start of Authority Record). It stores info about the server, admin, ttl  etc
- **Route 53 has a limit of 50 domain names, increased by contacting aws support**
- NS records: Name server records: Used by top level dns servers to direct traffic to content dns servers which has the authoratative information. so abcd.com will first go to top level domain, .com handler which gives a ns record. Which in querying will give a soa record. which contains the dns deatils. 
- DNS has a A record, whihc is the address record containing details of the address
- **Cname: A canonical name, used to resolve one address to another**. So that we can point multiple address to same location. like m.abcd.com and mobile.abcd.com pointing to same address
- **Alias records: used to map resource record set in our hosted zone to elastic load balancers**, s3, cloudfront etc used as website. Its like cname , where we can map one dns name ( like abcd.com) to another targer (s3 bucket)
- **Cname cannot be used to naked domain names( zone apex record) like abcd.com. it must be a A record or alias**
- Routing policies in aws: simple , weighted, latency, fail over, geo location, geo proximity routing, multi value answer routing.
- Simple Routing: One dns multiple ip. Aws returns ip in random order. You can have only record here.
- Weighted routing: Split traffic based on weights assigned, like 10% to us-east-1, rest to eu-west-1. We can set health checks on indiviail records, and if its fails, it will be remvoed from route53, until it comes back up.
- Latency based: Route based on lowest latency to end user. We create a latency resource record set. When route53 recevies a query it returns a selected latency record set to user.
- Fail over Routing: MOnitor primary site and when it fails, route it to secondary
- Geo location Routing:  route based on location of user, like 
- Geo proximity : Route traffic based on location of user and resources. We can also optionally choose to route more or less record to a specific resource by specifying a bias. To use this we must use Route53 traffic flow.
- Multi Value Routing:  Return multiple values in response to dns queries. similar to simple routing, but we can set health checks here, unlike simple routing and return only healthy values. 

### VPC
- 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 allowed private ip address range
- vpc peering is connecting machines in different vpc, we can peer machines in vpc in different accounts as well. peering is a star configuration. we can peer across region as well. **we cannot have transitive peering**
- in vpc , **1 subnet is equal to 1 availability zone**.
- security groups are stateful, while network access control list are stateless
- by deafult creating vpc creates a SG, NACL and Router
- in a subnet 5 address are reserved by aws
- we can have only one Internet gateway attached to a vpc
- **security groups cannot span vpc**
- **creating a load balancer in vpc, we need atleast two public subnets**
- vpc flow log is data about traffic flowing in and out of vpc. Stored in cloudwatch or s3. They can be created at vpc level, subnet level or network interface level. 
- we cannot creat flow logs for a vpc that is peered with a vpc, unless peer vpc is also in our account. after creating a flow log, we cannot change its config like a new iam role.
- bastion instance a hardned instance that allows us to secure our network, like jump boxes. Cannot use nat gateway as bastion host.
-**By default, any user-created VPC subnet WILL NOT automatically assign public IPv4 addresses to instances – the only subnet that does this is the “default” VPC subnet**
**NAT**
-nat instances needs source/destination checks disabled. Also performance depends on nat instance size. they are always behind security group
- nat gateway is highly available resource .**can have only one nat gateway in an availablity zone**. not associated with security groups.
**Network Access Control List**
- on creating a new nacl, all inbound and outbound are denied
- but default nacl that comes with a vpc allows all traffic.
- each subnet must be associated with a nacl. If we dont do this, its associated with default nacl. **Subnet can be assocaited with only one nacl at a time.**
- nacl are stateless. outbound and inbound can have different rules and ports
- rules are in down incremental order of preference. like rule 100 and higher prefernce over rule 400
- Security groups act like a firewall at the instance level, whereas NACL act as an additional layer of security that act at the subnet level.
**VPC Endpoint**
- allows us to connect to aws services from vpc without internet. So intsances doesnt need public ip to connect to aws services.
- vpc end points are vitual end points, scalable and highly available
- two types: interface and gateway endpoint
- interface endpoint is a elastic network interface with a private ip taht servers as entry point
-**gateway for s3 and dynamo**

### Elastic Load Balancer
- three types, application, network and classic
- application: best for http and https traffic. Operate at layer 7, application aware.
- network: for tcp traffic where extrem performance is needed , at layer 4.
- classic: not application aware. can balance by http or https, operate at level 7 and 4
- x-forwarded-for: when an ec2 behind elb get source ip, it will be that of elb. To get that of user, elb will add user ip to x-forwarded for header so that ec2 can get the actual ip
- load balancers have their own dns name, we are never given an ip to access the load balancers
- **classic load balancers allow for sticky sessions. application load balancers also allow this but traffic is sent to entire target group.**
- cross zone load balancing: if one zone has more bandwidth send traffic from other zones to this zone. we need to enable this. 
- **we can have path based routing that route based on urls.**

**Auto scaling group**
- to create an auto scaling group(ASG) we need a launch configuration first. WE can specify a bootstrap script for launch config.
- in asg we can choose, assign public ip to all, not to all etc while configuring it.
- after creating a launch config, we can create an ASG from it. in that ASG we can tell the scale out parameters and values.
-**instances created by ASG are spread out over all availabilty zones configured.**
- a sample q: we need min 6 instances and 1 availablity zone. For that we need to config 3 instances and 3 availablity zone. We if do 3 instances and 2 availabilty zones, if one zone goes down, we dont have min 6 ,but with 9 total if one goes down we still have 6.

### SQS
- it a distributed queue messaging system
- sqs is always pull based
- **messages can be upto 256kb in any format.**
- message retained from 1 to 14 days, with 14 being default
- two types: standard queue and fifo
- standard: unlimited no of txns, guarantee that message is delivered at least once.gives best effort ordering
- fifo is ordered. exactly one time processing. Fifo can have message groups, that is limited to 300tps max but has all features of a standard queue
- visibility timeout: a time for which message is invisible after a reader picks it up. If the priocessing is nto done and message deleted before visibility timeout, message becomes visible again and can be picked up by other jobs. its is max of 12 hours. 
- short polling is retrieve and if not found timeout Long polling is wait till we get a message or end of long polling timeout period.

### SWF
- Single workflow service. Helps to co ordinate work across distributed applications. 
- it is reprsented as tasks 
- swf workflow can live upto 1 year max
- its a task oriented api
- swf ensures task is assigned only once and not duplicated and keeps track of all our tasks. which are nto there in sqs
- swf has task starters, deciders and actors.

### SNS
- simple notification service
- its push based. it can send sms and emails. 
- it allows groupind receipients into topics. One topic can have multiple endpoint types
- it has inbuilt redundancy and availability

### Elastic Transcoder
- its a media transcoder in cloud. 
- pay based on minutes we transcode and resolution

### Api Gateway
- fully managed, secure api service . can expose https endpoints via rest
- we can cache responses from api gateway, which is deleted based on ttl

### Kinesis
- pronounced key-ne-sis
- it has kinesis streams, firehouse and anayltics
- **storage by deafult for 24 hours upto 7 days**
- streams can have shards.

### Cognito
- web identity provider gives access to aws after succesfully authenticating with a provider.

### serverless
- lambda scales out not up
- rds is not serverless, but **aurora is serverless**S
- xray allows us to debug our serverless. Rds hence cannot trigger a lambda
