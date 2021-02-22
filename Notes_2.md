### Region
- regions  have availability zone. **Region us-east-1 has AZ as us-east-1a, us-east-1b etc**
- each az is a physically separated from each another. 
- most region is built based on at least 3 AZ

### IAM
- policies in iam are written in JSON. They define waht each of the users, groups, roles can do. 
- IAM is global so users are created globally.
- after creating users we should create an accoutn alais, which gives a unique signin url for users to sign in. defaut sign in is for root user and should not be shared.

### EC2
- Security Group control access to ports, authorized ip range and control of inbound and outbound network
- **Security gruops are locked down to a region/vpc combination**
- Misconfigured SG leads to timeout error, permission error due to key issues and refused due to application/firewall issue.
- **SG can refer ip, cidr, other SG but not DNS name.**
- SG by default all inbound traffic is stopped and outbound is allowed
- SG can refer other SG. So we can set up a rule like on SG to allow connection from all other instances with a specific SG attached. So we dont worry about ip addres to access control any more.
- **Public ip can cahnge if we start and stop an instance. Elastic ip is when we need a static ip. its our own until we delete it.**
- **By default we are limited to 5 elastsic ip per account**. Better is to use a public ip and assign a dns to it.
- we can bootstrap instances using user-data. it runs with root user automatically.
- On demand: Usual instance, Reserved: for specific tasks, Convertible reserved which can be modified to larger isntances, scheduled reserved for specifci time, spot instances: bid and get it, dedicated instances: no other customer will share hardware, dedicated hosts: control entire physical server.
- On demand: billed per usage. Good for short term and when we cannot predict our work
- reserved: 75% discount compared to on demand: need to pay upfront. **Reservation can be 1 to 3 year and need to say what instance type.** Convertable is same bt type can be chagned. Little more costly. Scheduled is same but in time scheduled already. 
- spot: uptp 90% compared to on demand. Wil get it as long as our bid is within market range. If its below, the instance is deleted within a 2 min notification. We can make persistent request, where as when price drops we are again assigned an instance. 
- **dedicated: alloted for a max of 3 year period.**

**Instance Types**
- R: lots of ram ( in memory db), C: lots of cpu (compute/db), M: between r and c (general purpose, like webserver).
- I: lot of IOPS (DB). G: gpu( video rendering/machine learning)
- T2/T3: burstable, we get a good performance for a burst, but over use it means we lose the capacity. So it performs usual average most time and can burst its capacity when neeeded, but with a limit to no of bursts. There is a burst credit and if all credit is gone then cpu becomes bad. If machine stops bursting credits are again accumulated.
- T2/T3 unlimited: Unlimited burst. Be careful as this can lead to high costs.
- ec2instances.info website to find out which one to use.

**AMI**
- **They are not global. Are built for a region.**
- AMI published for a region can only be seen in that region. For other regions we need to copy explicitly to that region and launch instances from that. So amazon ami that we get on launching ec2 are also region specific .if we log into differnt region we might seee differnt ami.
- Private AMI are stored in s3 and hence take up space.
- By default AMI are private and locked for our account/region. We can make them public and share as needed.
- We can share AMI with another accunt, but we still remains the owner. But we some one copies a shared AMI then they becomes the owner. **To copy AMI the owner should grant read access on Ami storage, on the ebs snapshot or assocaited s3 bucket.**
- but it can be circumvented like wiht out copy permission. Share ami, launch instance and then create our own ami from that. 
- **You can't copy an encrypted AMI that was shared with you from another account.** Instead, if the
underlying snapshot and encryption key were shared with you, you can copy the snapshot while reencrypting
it with a key of your own. You own the copied snapshot, and can register it as a new AMI

-You can't copy an AMI with an associated billingProduct code that was shared with you from another
account. This includes Windows AMIs and AMIs from the AWS Marketplace. To copy a shared AMI
with a billingProduct code, launch an EC2 instance in your account using the shared AMI and then
create an AMI from the instance.

**EC2 placement groups**
- We can tell aws how to place instances
- cluster: instances in a single AZ in a low latency group. But if racks fails, all instances will fail. so increased risk. Used for big data job or very fast app with great network performance. 
- spread: instances in different hardware. max of 7 instances per group per AZ. for critical apps. But here instances can be spread across multiple AZ. Reduced risks here. Uses for max availability apps.
- partition : spreads instances across differnt partitions in differnt nracks within an AZ. Scales to 100 of instances per group (Cassandra, hadoop uses ). **So here unlike spread instances are not isolated but partitions are isolated. Here witin AZ we can have upto 7 partitions in a group.** partitions are independant of failure from each other.

### Scalability
- vertical scalabilty has a limit based on hardware limitations
- high availability can be passive, like multi az rds instance for read.
-**load balancer can handle https termination, sticky cookies , separate public and private traffic etc.**
- ELB is a managed load balancer from aws. it has a ui to help us
- three types: classic, application and network. Application and network are recommedned now.
- application load balancer: Works at layer 7 (http). can hanlde multiple apps in same machine or across machines.**can balance based on url or host name**. **suited for containers and micro services**. **has a port mapping for dynamic port**. we required to create one classic LB per app earlier ,which now needs only one app LB.
- app LB: we have a set of applications runnnig behind an application LB. Each such app can have many instances and health checks. we call these instances a target group, which is basically a group of instances for a single app.
-**we can enable stickiness at target group level here. Done at ALB and not at individual app level.**
- ALB supports http, https and websockets. app servers dont see ip of clients. True ip is instead set under x-forwarded header.
- **ALB is great for containers.** 
- NLB: at layer 4 (tcp). High performance, milions of requests per second. Extreme performance. it has same concept of target groups, but routing at tcp level.
- **NLB unlike other two can see client ip**
- all load balancers has a static hostname that gives us an url to use. we cannot resolve that url to an ip. 
- 4xx are client errors and 5xx are app errors.
- we can add a SG rule to our instance like only allow traffic from a SG in LB. so then we can access it only via LB and not directly from ec2. This is also useful cause we dont get acutal ip at instances behind LB, so we cannot restrict by ip range.
- **stickines works for classic and app LB**. it works via cookies, and those have expiration times. It can be enabled via ui, even after creating a LB. 
- using stickiness can cause un even load. Cause one instance may be busy serving a long running customer, causing LB to send other traffic to other instances, increasing load in them.
- LB can hanlde SSL certificates. use x.509 certificate which can be managed by ACM (aws certificate manager). we should give a default cert for a https listener here
- IAM roles assigned to ASG will be attached to its instances. We dont pay for ASG only for instances behind it. ASG can also terminate unhealthy instances. 
- **ASG can span multiple AZ.**
- **Network Load Balancers expose a public static IP, whereas an Application or Classic Load Balancer exposes a static DNS (URL)**
- **SNI (Server Name Indication) is a feature allowing you to expose multiple SSL certs if the client supports it**.Your application load balancer is hosting 3 target groups with hostnames being users.example.com, api.external.example.com and checkout.example.com. You would like to expose HTTPS traffic for each of these hostnames.

- Default AZ termination policy: First AZ with most instances, and if multiple instances, delete one with oldest launch configuration.
- ASG has a cooldown period. Like a wait period for a previous scale activity to finish before triggering another. this helps our instance metrics to come back to normal levels. We can also set our own cooling period in simple scaling policy. So if default cool down (300 sec) is too large for our case, we can reduce them. Also we can fine tune the cloudwatch alarms and cooling period to manage how often instances are being scaled in or out.

### EBS and EFS
- EBS is a network drive that can be attacehd to an ec2. It can be detached and attacehd to another instance
- **Its locked to an AZ.** To move we need to snapshot and then move it to another AZ
- **we get billed for provisioned capacity and not for how much of that we use.**
- we can increase volume over time.
- EBS types: GP2(SSD): General purpose ssd that balances price and performance
- IO1(SSD): highest performance for super critical apps
- ST1(HDD): low cost for freq accessed, high through put
- SC1(HDD): cold volume for less freq accessed tasks.
- EBS volumes are characterized by volume, iops, through put etc
- Only GP2 and IO1 can be used a boot volumes.
- EBS drive is not mounted by deafult. We need to mount it to use. Also EBS wont have a file system at start. We need to create one and then mount it ( for new ebs volumes). exisitn one will have a file system. Also we need to add a config to the instance to mount the ebs on every start. 
- GP2(SSD): for most workloads. boot, virtual desk top, low latency, dev envs etc. 1gb to 16TiB. Max iop is 16k. We get 3 iops per gb (3:1), max iops is at 5334gb. Min of 3 iops and burst of 3000 iops. Burst is only when we have less than 1000gb.
- IO1(SSD): if we need more than 16k iops. like critical db. 4GB to 16TiB. it has provisioned iops. min 100 to max 64k( for nitro instances), else 32k max for other instances. Each gb can have 50 iops. (50:1). So we cannot have a small volume with a huge iops, it has to be within the ratio,
- ST1(HDD): fast through put low price (streaming workloads). Big data, data warehouse, log, kafka, etc 500GB to 16TiB. max iops of 500.max thoughput of 500MiBs and can burst (400MB for TiB)
- SC1(HDD): lowest storage cost. 500GB to 16TiB.max iops of 250. max throughput of 250 and can burst.(12mb for TiB)

- snapshot: incremental. And **backup uses IOPS**. SO dont run them whem our app is not under high load. Are stored in S3, and are billed but we cannot access them. Not necessary to detach volume to get snapshots, but good to do that. Max 100,000 snapshots per acccount. Can copy snaphosts accross regions. **EBS volumes restored from snapshots needs to be prewarmed.** **Can be automated snapshots for Amazon data cylce manager**(max 7 of an ebs volume can be stored). We can create an AMI from a snapshot itself.

- Encryption: when we create an encrypted ebs, Data at rest is encrypted. All data moving between instance and volume is also encrypted. snapshots are encrypted, all volumes created from that snapshot is also encrypted. All encrypt and decrypt handled by aws. It was very minimal impact on latency. and uses KMS. While copying an unencrypted volume we can enable encryption.

- instance store is ephermal store and physically attached to an instance. it has better i/o compared to ebs. Data survives reboot but lost on termination. Some devices come up instance store instead of a root ebs volume. **we cant resize instance store.** and we need to take back ups.

- **EBS is alread redudant as its replicated within an AZ**
- EBS has RAID 0 and 1. RAID 5 and 6 are  not recommmended for EBS
- RAID 0: performance. Combines two or more volumes into one big logical volume. So if one disk fails our entire data is lost, but we get high performance. So used for apps with high IOPS and low risk. If we combine two 500GB ebs volumens with 4000 iops each , we get 1000GB with 8000 iops here.

- RAID 1 :Fault tolerance. Here we have one large logical volume but we write same data into both underlying volumes so taht we have a backup. So low performance but high reliability. Same example above create a 500GB 4000iops here.

- EFS : Its a managed file system. (network managed). can be mounted to multuiple instances. Available over multi AZ. High availabilty scalable. expensive. Use NSFV4.1 protocal. Use security group to control access to EFS. **Only compatable with linux ami**. It can be run in general purpose or max io mode. We can use efs file sync to back up local to efs, we can also incremental backup of efs to another efs. Also encypt at rest using KMS. EFS can be run in bursting or provisioned iops mode as well.

### Database
- rds has 6 options. aurora, mysql orcale, postgres, maria, sql server. RDS name needs to be unnique across a region.

- **RDS has read replicas for read scalability. upto 5 read replicas. They can be within same AZ, cross AZ or cross REgion**. Reads are eventually consistent.  Replicas can be promoted to own DB. Apps needs to change to use the read replicas. ( diff conn string for each replcia and db). replication is async in read replicas

- **for disaster recovery, we will have multi db, in diff AZ, but will have only one conn string. It has a failover to connect to backup instance on errors. Replication is sync here. So limited scaling.**

- backup are auto enabled at rds. Can restore to any point in db. **Stored for 7 days be default to max 35 days for auto back ups. Manual backups can be stored for any duration.**

- encryption at rest using KMS. can use SSL certificate to encrypt data on flight. To enfore ssl set rds_force_ssl=1 in postgres and in mysql run a statement to require ssl. Connecting to db via ssl doesnt mean that ssl is enforced.

- rds has SG for security. and are usually in private subnet. we can log to rds using tradional username/pasword or iam users( for mysq and aurora). Token in this care expires after 15 mins. For iam login we should use ssl.

- aurora : not open source, cloud optimzed(5x mysql, 3x postgres), compatable with mysql and postgres. Storage grows auto by 10gb to max of 64tb. **max 15 replicas compared to 5 in mysql. Failover is instantaneous. Multi az, high availability.** Costs more than other RDS

- encryption at rest can be done only when we create the first db instance.  TO encrypt non encrypted , we follow same procedure as EBS. check ports and rules in DB SG.

- **RDS supports transparent data encryption for DB encryption. Only for Orcale of SQL server**. TDE can run on top of KMS but affects performance. 

- **Aurora keeps 6 copies of data across 3 az. 4 copies out of 6 needed for writes and 3 out of 6 for read**s. It has self healing with peer replication. Storage is across lots of volumes.

- one instances takes the write (master), if master fails, one is promoted to master. read replicas supports cross region replication.  Read replicas have auto scaling. it provides a reader endpoint like write endpoint to read. So read is load balanced at connection level not statement level though. 

- Aurora has all cool features of all other rds in aws plus extra. Like it supports iam login.

- **aurora gloabl database, span regions and has one primary region and one disaster recovery region.**

- aurora serverless .. no need to choose instance type, and cannot predict .supports mysql 5.6 and postgres(beta). can migrate from aurora cluster to serverless and vice versa. Aurora capacity units is the unit for measuring aurora serverless usage.  Billed in 5 mins increments of acu.

- caching helps make app stateless, like storing session.
- **redis has persistance to reboot**. it has sorting, pub-sub, distributed, read duplicates, multi az, encrypt at rest
- redis suports redis auth (username/pass), SSL in flight is supported.
- memcached supports SASL
- **memcached has no reboot presistance.**
- elasticache is aws term for cache which has two flavour redis an memcached
- **neither cache supports iam auth**
- write through: any write to db write to cache as well
- lazy load: stale data in cache
- session store with ttl

### Route53
- Managed DNS. in aws there are four main DNS. 1) A url to ipv4, 2)AAAA url to ipv6 3) cname url to url 4) alias url to aws resource. 
- route53 has load balancing, routing,  health checks.
- costs .50$ per month per hosted zone. ITs a global service.
- route53 or any dns can send a ttl with a record lookup. the browser can cache the record for that period. and query again once its expired.
- aws resource url are very long and we may want it to be our own address. we can use cname to map that url to any url that we want.**cname is only for non root domain. so url we map cannot be mydomain.com (which is root), but should be aaa.mydomain.com or some subset**
- **Alias can also do above. It works for both root and non root domain.** Alias is free of charge and support for health checks. So Alias should be used for load balancers cause we can point root domain to it , costs nothing and supports health checks
- Simple routing: Use to redirect url to a resource or ip. **We cannot have health checks.** But we can return multiple values and client chooses one at random.Like one url, returns one or more ips.

- Weighted : Percentage of routes to a particular resource. Sum of % doesnt have to be 100. Can have health checks. 

- Latency: redirects to resource with least latency. Latency is calcauted based on latency of user based on aws region. So a user in germany may be low latency is to us and hence routed there. 

- health check : deemed unhealthy if it fails 3 health cehcks in row, reverse for healthy. **deafut interval for health check is 30s, we can set it to 10s for high cost.** We can have http, https, tcp health checks. **Https has no ssl checks though**. health checks can be linked with dns queries. Pay additional for extra features on health checks, checks on non aws end points etc. can link them to cloudwatch.

- failover routing:**needs mandatory health checks**. route if one instance fails health checks. **Can hve only one primary instance and one secondary instance**

- geo location routing: Route based on user location. also needs a default policy to cover users with no geo rules. Geo can be country or continent or default. 

- multi value: route to multiple resource and associate route 53 health checks with them. Upto 8 healthy records returned for a route query. its not a replacement for ELB. We can send multiple ip for a same ip.  based on health cehcks route53 will choose 8 values to return.

- route53 is also a registrar for domains. we can use a 3rd party registrar with route53. for this we create a hosted zone and update teh Name server (NS) records on 3rd party for route53 name servers. Hosted zone needs to be public as its for external company. private hosted zone is for internal use. 

### Generic Notes
- **ELB cannot have a A record(ipv4) cause its ip cahnges often. INstead it has an alias record.**
- best way to store session/user data. is to send a session_id from user and store data against taht in a cache. asking web client to send session in cookie every time is risky and wrong.
- we can use read replicas or write through (write to cache on write to db) to increase read performance.
-A**WS Elastic Network Interface is simply a virtual interface that can be attached to an instance in a Virtual Private Cloud (VPC).** Like attach EFS to instances.
- Reserving instances can save cost for us

### BeanStalk
- for ec2 we have a golden ami: which has all apps and everytin required installe. create an ami from that and use it in future. Or we can use user data if installation is small ,for bootstrap.
- we can have a hybrid of above two ( which is concept of beanstalk)
- for RDS and EBS we can restore from snaphsots to speed up our instances. 
- Elastic beanstalk helps with deplyoing wihtout hassle. It configures and brings up resrouces for us based on cofnig .Its a managed service. BS manages OS and deployment strategy etc.

- Here developer should be concerned only with teh code. 
- it has 3 deployments : **Single instance ,LB+ASG( great for prod or pre prod web apps),  ASG only (great for non web prod apps like worker apps)**
- BS has three components : Application, Application Version and Environment name.
- we can deploy app version and promote app version, also rollback if needed. We have full control over app life cycle.
- **if BS doesnt support a platform we can write a custom one for it.**
- BS is like an auto configured cloudformation. we can configure it using ui.

### S3
- buckets need to have globaly unique names, but created regionally
- **bucket names should not have uppercase or underscore and 3-63 chars long, must start with lowercase or number**
- an s3 key is full path to file. there are no directories even though we get a sense of that. Its just creating keys separted by slashes. 
- max size is 5TB, if uploading 5GB or more need multi part upload.
- we can open a file in s3 by right click and open or by using public url. First step needs just aws user permissions while second one needs the file to be public visible.
- version needs to be enabled at bucket level. So when we upload version is increaed.  with this we are protected against delete and can be rolled back . Any file before versioning will have version as null. Delete puts a delete marker against versioned file.
- **Explicit DENY in an IAM policy will take precedence over a bucket policy permission**

- there are 4 ways to encrypt in s3: SSE-S3 : encrypt using keys managed by aws, SSE-KMS: use aws KMS, SSE-C: client provided encryption keys and Client side encrypted.
- SSE-S3: Object encrypted server side, AES-256 encrypted .Must set header x-amz-server-side-encryption=AES-256
- SSE-KMS: we can control key rotation and audit. Object encrypted server side Must set header x-amz-server-side-encryption=aws:kms
-SSE-c: aws doestn store the key. HTTPS must be used. key fully managed by user key must be in header for every request.
- Client side: Use library such as aws s3 encyrption client. encrypt and send to s3 and decrypt when retrieving
- encryption in transit: aws s3 exposes http which is not encrypted, and https where data is encrypted on transit. we can use either, though second is preferred. in flight encryyptin is ssl/tls
- we can set default encryption for a bucket.

-**two types of s3 security: iam based and resource based.**
- resource based:  **bucket polices ( can be cross accout), object access control list (fine control) and Bucket access control list (less common)**
- so total 4 ways of securty control in s3.
- **bucket polices are json docs. it has four parts: Resources(bucket), actions (set of api to allow or deny), effect( allow/deny), principal(account or user to apply to)**
- S3 supports VPC end points. it has s3 access logs stored in other s3 buckets , api calls can be cloudtrailed. we can also have MFA based acccess control for versioned buckets. Also use signed url for access.

- S3 website: < bucket-name>.s3-website.< AWS-region>.amazonaws.com.
- will get 403 if public access is not enabled. 
- **without enabling website access, a file in s3 cannot refre another file in s3, like html refer image.**

- **we need CORS enabled to get data from another s3 bucket.**

- s3 has read after write for put action for new objects. **But if we do a GET before put succeeds (404), next GET might be 404 as well due to cached results.**
- eventual consitency for delete and puts of existing objects

### Generic Notes
- **iam polices are not immediate. they make take a little bit of time to be applied**
- inline polices cannot be added or attached to other resources, for that use managed polices
- ec2 metadata: can get ec2 metadata from within ec2 itself without a role. 
- url is http://169.254.169.254/latest/meta-data/. Slash is very important at thhe end.
- using this we can retrive iam role name but not the iam policy. here latest is version number. Latest info has three components, Meta-data, user-data and  dynamic.

### Advanced S3

-s3 is global but buckets are regionalized.

- to use s3 mfa delete, needs versioninig enabled. need mfa to permanently delete a version or stop versioning. we dont need mfa for enabling version or seeing version. Mfa can be enabled only by bucket owner(root account) and mfa currently works only from cli

- old way for s3 encryption was to create policy and refuse all requests without encryption values. new way is to enable default encrytion in s3. but b**ucket policies will be evaluated before s3 encryption settings though.**

- we can log all requests to an s3 bucket. logs might have a small delay to be logged

- cross region replication: **source and destination needs versioning enabled and buckets must be in diff aws regions. buckets can be in diff accounts as well.** copy is async and must have proper iam permissions. We can create rules for replication like replicate by prefix or time etc. **We can even change storage class or ownership on replication.**

- pre signed urls can be created using sdk or cli. by default pre signed url expires in 1 hour, which can be extended by using --expires-in flag. users with pre signed url inherits owners put and get permissions.

- cloudfront can provide ssl encryption at edge over ACM and can over only https option. it also supports rtmp protocal for media.

- we can create an origin acces policy in CF so that we can access s3 objects only via CF via this identity. this creates a policy in bucket that only access via the origin access id created. so a user is createf for CF and only that has access to s3. dns propogation of CF and s3 link might take a few hours to work. 

- if s3 can be accessed only via CF we cannot use s3 signed url for access enabling. So we need to use CF signed s3 urls. which can set list of ip permitted, url expiration, trusted signers(other aws accounts etc). This can be done only via SDk and we have validate req in our app. we can genearte any number of signed cf s3 url. Url expiry is subject to use case. How this works: Client or browser talks to our app. Our app talks to CF which has edge location linked to S3 via identity access origion. Our app then geneates a signed url and send to client which uses it to access the content. So this signed url is different from pre signed url from s3.

- CF uses a global edge network and has ttl for cache. so great for static content. S3 cross region rep is instantaneous and is great for dynamic content and is read only.

- we can access who can access CF location based on geo (country). geo restriction. It has whitelist of countries to allow or black list to deny.

**S3 Tiers**
- s3 general purpose, s3 standard -infrequent access, s3 one zone -IA,  s3 reduced redundancy(deprecated), s3 intelligent tier and glacier.

- s3 standard: 11 9 availablity across multi az. it can sustain 2 AZ failure at a time.
- s3 RR: 11 9 availablity. Can sustain loss of data in single facility. for non critical, reproducible data at lower redundancy than standard. not recommended to be used.
- s3 standard IA: less freq accessed, but needs to be retrieved instantly when required.same 11 9 availability across mutli AZ. it has low cost compared to standard but costs for retrieval. can sustain 2 cuncurrent facility failure.for disaster recovery backup etc.
-s3 IA one zone: Same as above but one AZ. so if one AZ is destroyed data is lost. 99.95 availibility. Lower cost compared to IA. used for storing secondary backups, very low critical reproducible data etc.
-s3 intelligent tiering: same standards as s3 standard. small monthly monitoring and tier move fee.auto move between tiers.
- glacier :low cost storage. data retained for 10 years or more.alternative to on premise storage. cost 0.004/GB per month and a retreival cost. Stored in archiev upto 40TB and archives in vaults. 3 retreival options: expedieted(1 t0 5 min): 0.03 per gb and 0.01 per request, standard ( 3to 5 hours): 0.01gb/0.05 per 100 req, bult (5 to 12 hours): .0025gb/0.025 per 1000req.

- s3 RR has lowest durability of 99.99 , all other have 11 9
- s3 one zone IA has 99.5 availability, intelligent tiering has 99.9, galcier has NA all other 99.99
- RR has >=2 AZ, one zone  has 1 AZ, all other has >=3 AZ
- standard and standard IA has withstand 2 AZ failure, one zone 0 AZ failure, all other 1 AZ failure.

- we can set life cycle rules to move data between tiers. transition actions to move data, expiration actions to expire obejcts after a certain period. we can even set rules to delete incomplete multi part data.

- snowball is physicall data transfer solution for our in premise data. Secure tamper resistant KM 256 encryption. tracking using SNS and text messages, eink shipping label. Pay per data transfer job. snowball edge is snowball with computational facilities. supports custom ec2 instance or lambda functions. useful for preprocess data before moving.
snowmobile is a physical truck to move data.

- hybrid cloud, part of cloud in aws and part in own premise. How do we expose s3 storage to on premise? as s3 is proprietary. its done via a storage gateway. aws has block storage( ebs, ec2 instance store), file storage( efs) and object store( s3,glacier). storage gateway will bridge all these into our premise. use cases are disaster recovery, backup, restore etc. 3 types of storage gateway : file gateway, volume gateway and tape gateway

- file gateway :  configured s3 buckets accessible via NFS and SMB. supports standard, IA, 1Z IA. bucket access using iam roles for each file gateway.most recently data is cached locally. since nfs can be mounted to many servers at once.

- volume gatewy: **block storage using iSCSI backed up** s3.backed by ebs snapshot. it has caching for recent data (cached volumes) and entire data is on premise and scheduled backup to s3 (stored volumes). So EBS volume snapshot backed to s3, which is linked by a volume gateawy to our server via iSCSI protocal.

- tape gateway:  for tape backup. virtual tape library backed up s3 and glacier.backup using existing tape and iSCSI interface. So archived tapes in glacier or backup physicall tape in s3 linked via tape gateway to our premise.

- Athena: **serverless service to do analysis against s3 files.** we can query against s3. can use sql 	queries or jdbc/odbc. cahrged per query and data scanned. supports many format like json ,csv, orc, avro, parquet(built on presto)

### SQS,SNS,Streams
- sqs is a managed queue service. standard queue: scale effortlessly. retention from 4 to 14 days,no limit to no of messages and low latency. can scale horizontly with number of consumers. can have duplicates ( at least once delivery) and can have out of order messaging( best effort ordering). **Limit to 256KB per message size**.(body). Also body has to be a string.we can add metadata to messages in sqs. sqs is also charged for polls. so too many polls can be dangerous

- sqs delayed queue: delay message upto 15 mins. default is 0. **can set delay at queue level or at message level by setting DelaySeconds parameter.**

- **consumers can receive only 10 messages at once**. consumers can process messages within visibilty timeout and then delete it. So when a consumer pulls a message from queue its invisible for a period : visibility timeout. which can be 0 seconds to 12 hours (def is 30 sec). So if mesasge is not processed and deleted within the it will be visible again and can be consumed again. Too low Visibility other consumers pick it up, too high then other consumers have to wait for a long time to process failed message.

- we can use ChangeMessageVisibility to change visibility during processing and DeleteMesage to delete it.

- if message exceeds Visibility timeout it goes back to queue. If this repeats we can set a reDrive policy.  If message exceeds this threshold it can be send to a Deadletter queue. We have to create DLQ first and them remember to process messages in it before they expire from DLQ.

- Long polling: Short polling is poll and return immediately. Long Polling is wait for some time to get messages. Wait time can be from 1 sec to 20 sec. Long polling is preferential and 20 sec is a good time period. WaitTimeSeconds api to set long polling interval.

- **fifo queue name must end in .fifo** . It has lower throughput 3000/sec with batching and 300 without it. Exactly once. No per message delay, only queue level delay.  Can do content based dedup, 5 min interval for dedup using duplication ID. Can group messages into groups and one worker assigned to process one group.

- SNS is a pub sub.. Send messages to sns and all subscribers receive it. Now subscribers can filter message. 10M subscriptions per topic and 1M topics limit. Subscribes can be sqs, http/https, email (normal email and json), lambda, sms, mobile notifications. Lot of amazon servies integrae with sns and sends notifications.

-sqs sns fanout : Send message to a sns, which has many sqs subscribers. Now we can parallel proess same message in differnt consumers. Graet decouple, reliabilty and availablity. We can add or remove subscribers as needed, and delivery is guaranteeed and even can have dealyed processing.

- managed alternative to kafka. a streamign tool. data is auto replicated to 3 AZ.
- kinesis streams(kinesis):  low latency streaming ingest , kinesis analytics : perform analytics on streams using sql, firehouse : loads streams to s3, elastic search, redshift etc.

- streams have shards/partitions. retention is from 1 day to 7 days.  can reprocess and replay data. multiple applciations can consume same stream. Once data is inserted to kinesis it cannot be deleted (immutability).

- each shard has 1MB/s or 1000 writes per second. 2MB per shard per read. Billing is per shard provisioned. We can batch messages or calls to improve efficiency.. We can incorease or decrease shards. Records will be ordered in a shard.

- Put record in kinesis has a key and data. Key is hashed to determine the key. messages in a shard get an increasing sequence number. Choose partition key as varied as possible to aviod hot partitions.

- throws ProvisionedThroughPutExceeded exception on error. Solution is backoff, or check partition key is good or increase shards(scaling). 

- Kinesis Consumer Library(KCL) uses dynamo to track checkpoint and track other workers etc. to retrieve from cli use get shard iterator and then get record.

- we can use IAM to control kinesis access. Can use encrupt at flight using https. Encrupt at rest using KMS. Also encrypt/decrypt at client side VPC end poins also available.

- Kinesis analytics is amanged and is auto sacled. charged per actual consumption. Analytics can produce streams as well.  Firehouse has about 60 sec delay, has sacling. support different data format but need to pay for conversion. Also need to pay for amount of data flowing through firehouse.

- SQS, SNS are cloud native or propreitory to aws.  So if we want to migate open source queue to aws, we need to use Amazon MQ. its  a managed apachce activeMQ. Doesnt scale  like sns/sqs runs on dedicated machine. it has both sqs and sns feature. it supports a lot of open source protocols like wss, amqp, stomp, mqtt etc. As it runs on dedicated instances its controlled by Securoty groups for access control.


### Serverless
- serverless includes everytging where server is managed by aws, like lambda, dynamo, s3, api gateway , sns, sqs, kinesis, cognito, aurora serverless etc.

- lambda pay per request and compute cycle. 1M lambda req free tier and 400kGB of compute cycle. afer that .20$ per 1M requests. pay per duration is increment of 100sec. here 400k is for 1gb ram, if with less than 1gb we get lesser compute cylce free. after free tier we pay 1$ per 100k compute cycle. can have upto 3gb memory, more ram means more cpu and performance

- max timeout 30 mins now?. memory from 128mb to 3gb. Disk capacity for /tmp write is 512MB. concurrency limit by default is 1000. zip file 50MB max. uncompressed should be max 250MB. env variables max 4kb.

- we can deploy lambda at edge locations. pay for what we use. CF has four req/res. Origin req and res. Viewer req and response. **lambad at edge can send response even not contacting the origin. edge deployed lambda run globally.**

### Dynamo
-**managed distributed and replicated in 3 AZ**. integrated with IAM. event driven programming with dynamodb streams.each table can have infinite rows.
- max item is 400KB,data types: string, binary, boolean null, map,list, strnig set number set and binary set
- has provisioned throughput. 0.00013 per read unit, one read unit is one 4kb strongly consistent read or 2 eventually consistent read of 4kb.1 write unit is .00065$, and is 1 write of 1 kb/sec. we can increase througput using burst credit. can get ProvisionedThroughPut exception

- dax: dyanmodb accelator:  seamless cache for dynamo. each cache has 5 min ttl by default. 10 dax nodes per cluster and is multi AZ(so min 3 nodes needed). has encryption at rest.

- changes in dynamodb can be streamed. a cahngelog of every thing that we do.it can be read by lambda and processed. streams is needed for dynamo cross region replication. has 24 hour default data retention.

- dynamo has transctions since nov 2018. can co-ordinate txns across tables, can include upto 10unique items of 4MB data in total.
= on demand: capacity sacles on demand, no need to provision. but 2.5x more costly than provisioned.but good for spurts of usage request.
- **dynamo has encryption at rest using kms, iam access control, and vpc endpoints.** ecnryption at transit using ssl/tls has backup and gloabl tables.  aws has Dynamo Migration Service( DMS) to migrate to dynamo.

-A DynamoDB Accelerator (DAX) cluster is a cache that fronts your DynamoDB tables and caches the most frequently read values. They help offload the heavy reads on hot keys off of DynamoDB itself, hence preventing the ProvisionedThroughputExceededException

### Api gateway
-handle api version, env, great security etc. create api keys, req throttling, swagger integrated valdiate req, genearte sdk and api specs and inbuilt cache. in vpc api GW can trigger lambda and ec2 in vpc.

- resources in api gw are paths and methods are http methods to invoke them. even wiht nested resource, we need to deploy only once, and all resources are deployed.

- aws gw iam permissions: can link an iam policy to api gw to limit user access. Uses sigv4 capability where iam creds in headers. cannot be used outside aws as iam is within aws
- **aws lambda authorizer: use lambda to authoruize token in header. we can cache result of autorization. lambda must return an iam policy**
- aws cognito user pools: cognito has user pools and api gateway verifies against it. it only manages authentication and no authorization. can be external provider login like fb, google etc.

### Cognito
- it has user pools and identity pools( federated identity) and  cognito sync(sync data to cognito)
- cognito user pools (CUP): serverless db of user with simple user/password. can enable federated identitites.. can send back jwt and can be integrated with api gateway. can verify email/password etc for login, so we can use fb user/pass to login. Its like using fb accout to access aws
- cognito federated pool(CFP):  provide direct access to aws resuorce from client side. so we log into fb and then redirected to aws with an iam policy. unlike above we login to fb and gets temp creds to use in aws.
- cognito sync: depecrated use AWS AppSync instead.  requires Federated identity pools to work. cross device sync and offline capability. data stored in dataset of upto 1MB.

### Generic Notes
- aws STS is Secure Token Service. Can use Cognito to invoke STS and get temp creds.
- using DAX is great for performance. Or apigateway caching.
- **dynamo gloabl tables can reduce latency for geo users.**
- s3 transfer accelaration uses edge location to upload to s3 faster.
- s3 signed  urls are not efficient for gloabl url. CF signed is better. and also gives origin access protection.
- redshift is used for OLAP

### Databases
- neptune is fully  managed graph database.uptp 3 AZ, 15 read replicas etc.

- for rds we must provision the instances. we dont directly do it but tell aws to do it for us.  has read replicas and multi AZ, all usual security etc. RDS can have small downtime when maintanence happens or sacle up or failover etc.Also may need app cahnge when db cahnges. aws does os security while we need to take care of the rest. Perf depends on instance and read replicas. **RDS doesnt auto scale as we need to specify the instance**. 

- aurora has compatable api for postgresql or mysql. it has all missing features of rds. **Auto scale of read replicas and which can be gloabl, but we need to specify instance for aurora to be installed so write doesnt auto scale**. **Also there are global aurora.  We can have aurora serverless as well.**

- elasticache also needs instances to be specified. have read replicas.

- dynamo auto scales as required. 

- s3 is serverless and scales auto. its a key value store for large objects

- athena is serverless.

- **redshift is based on postgresql. Its for OLAP and not for RDS. Columnar data storage.**

- elasticsearch has cluster, for big data especially for search. multi az,

### Cloudwatch and Cloudtrail

- CW provides metrics for all services. MEtrics are under namespaces. Dimensions are attributes of metrics.
- upto 10 dimension of a metrics for free tier . metrics have timestmaps
- **ec2 instance metrics are every 5 mins. detailed monitoring is 1 min metric, better auto scaling.**
- **ec2 memory usage is not default pushed.** Must be pushed as a custom metric.
- Default metric is 5 min(basic). **Detailed metric can be upto 1 min. High resolution is only for custom metric.**
- **custom metric has def duration of 1 min but can go down to 1 sec.**
- use PullMEtricData to send custom metrics.
- **CF dashboards are global**. So we can include graphs from diff regions in one dashboard. can set up auto refresh upto 15 m. 3 dashboard ( upto 50 metric) free. after than 3$/dashboard/ per month. 
- CF can collect logs from lambda, vpc flow, cf log agents, bean stalk, ecs, api gateway, cloud trail, route53
- we can batch export CF logs to s3 or stream to elastic search
- CF logs have log groups( any name) and log stream ( for instance , fn etc). We can define log expiration ,we pay for data storage in CF. make sure iam permission to send logs to cf. logs can be encrpted using KMS
- insigts can be used to query CF logs.
- in ec2 set /etc/awslogs/awscli.conf to point to CF to send logs.
- **CF alarms have 3 states: OK, Insufficent_Data, ALARM.** High resolution custom metric can be evaluated only for 10sec or 30 sec.s
- can schedule cron or pattern match to trigger cf events. 
- CloudTrail provides audit governane complaince etc. ITs enabled by default.

### Security
- encryption in flight (ssl) using certificates.this avoids Man in Middle Attack
- aws manages KMS keys. KMS is a store and we have basic control over it. its integrated with IAM for access control.its fully integrated with many aws services.
- KMS has a Customer Master Key(CMK). we can never see it. KMS manages it and rotates it for security.
- **KMS can encrypt upto 4kb data per call. For more than 4KB use a technique called Envelope encryption**. to give access to kms make sure key policy allows user and iam policy allows teh api calls.
- KMS allows us to create, rotate and manage key. CT can audit them for us
- Three types of keys: Aws managed CMK( free), user created in KMS(1$ month) and user imported key to cms(1$ month). also pay 0.03$/10k for api calls for encrypt and decrypt using kms
- we sent an encrypt/decrypt req, kms checks if requester has iam permission to do that first. 
- **EFS, EBS, RDS, Elasticache needs snapshot/backup for encryption using KMS.** S3 allows inplace encryption.
- when we enable encryption in a aws service, aws creates KMS keys within KMS store.they are aws managed keys. We need to use Customer managed keys for our use of KMS. 
- KMS creates an IAM policy on key creation for access.

- **AWS parameter store (Simple Systems Manager :SSM) to securely store your config. It has optional seamless encryption with kms.** Its free and scabalbale and has a easy api to use. has version tracking and iam integration. has CW integration and CF integration. We can store either plaintext or encrypted data.

- SSM can get all parameters under a path including all sub hierarchy params (tree structure). great for prod/dev parameters.
- For SSM decrypt and retrieve, user needs to have IAM permisssion in KMS
- SSM Parameter Store has versioning and audit of values built-in directly. KMS doesnt have audit permissions
- Secrets manager has key rotation, password geneariton, cross accoutn sharing and is not free when compared to SSM

- STS ( Security Token Service)
- grants temporary access, with tokens valid for 1 hr. Gives cross account access. Had federated access and other provides access.

- AssumeRole: We define an IAM role to use by other aws account. then define which external aws account can access that iam role. Then use STS to retrieve creds and impersonate that iam role for external account. Temp creds from 15 mins to 1 hr. So STS ends tem creds for us to assume that specific role.

- we can use identity federation access( ldap, active directory. saml, single sign on, cognito, openid ) to authenticate the user adn then once authenicated use sts to get temp creds and access aws resources.

- saml for basically used for enterprise auth like via active directory. So we login  to our auth provider ,whcih gives a saml assertion, which we trade with sts to get temp creds. or we can use that saml assertion to validate ui login and get access to console.

- custom identity broker is used when we dont have access to saml 2.0. Identity provider we ahve to program and has to determine the appropriate iam policy to grant access.

- aws cognito federated identity pools: We log into identity provider(3rd party) get a token and send it to cognito, whcih verifies token with identity provider and gives temp access. Here we connect to public identity provider.


### VPC
- CIDR subnet masks shows how many bits we can change in ip. /32 means 2^0 = 1 bit changale so 1 ip possible. /31 is 2^1 and /30 is 2^2 and so on.
- default vpc has internet access, get public/private ip for all instances.

- **you can have upto 5 vpc in a region** (soft limit that can be increased) **Each VPC can have upto 5 CIRD**. Min range is /28=16IP and /16=65536 IP. 
- VPC allow 10.0.0.0/8, 172.16.0.0/12 and 192.168.0.0/16
- subnet are tied to specific AZ. We can create subnets in different AZ for high availablity. Subnet acts like AZ for aws apps.
- aws reservers 5 ip in each subnet. First 4 and last. First for network address, second for vpc router, third for amazon provided dns mapping, fourth for future and last for network broadcast.
- IG scale horizontaly, high available and redundant. One vpc can be linked to only one IG. IG acts as NAT instances for instances with public IP. for IG to work route table needs to be updated.
- NAT allows private subnet to interent. Must have elastic ip. Must disable source/dest check. Must be lauched in public subnet
- NAT instance is not auto scaled, depends on ec2 size for performance, not high availablity , need to maintain security groups etc.So use NAT gateway.
- **NAT gateway cannnt be used by instance in same subnet.** Requires an IG. Created in specific AZ with Elastic IP. pay per hour and bandwidth. 5gbps with scale upto 45gbps. No SG to manage

- enableDNSSupport: Default true. Use aws dns at 169.254.169.253. 
- enableDNSHostname: **Default false for private vpc and true for default vpc**. Wont do anything unless enableDNSSUpport is also true. it will assign public hostname to instances with public ip. If in private hosted zone needs both params to be true to resovle that zone.

- NACL at subnet level and SG at instance level. SG are stateful ,so if inbound passes, outbound is passed as well. NACL are stateless, separate inbound/outbound.
- One NACL per subnet, default NACL in aws all traffic allowed, new NACL denies everything. NACL rules has number, lower no with higher precedance. Last rule in NACL is * and deny all whcih cannot be cahnged. Increase rule no by 100 as recommended by aws. NACL is great at blocking specific ip at subnet level. SG we can allow traffic from specific ip not other way around.
- **NACL is auto applied to all instances in the subnet**

- VPC peering allows to conect two vpc privately using aws.make them behave like one network. But CIDR musnt overlap. 
- VPC peering is not transitive.
- **we must update route tables in every peered account.**
- a peering request should be accepdt by all vpc to start working
- after peering we need to add routes to peer in both accounts, route points to a peering connection.

- vpc end point allows us to access aws resources from a private end point. for this as well we need to update route tables.**vpc end points scale horizontly.**
- two types: Interface, which has a Elastic Network Interface( Private ip) and needs a SG: for most services. **Gateway, provisions a target and must be in a route table : for s3 and dynamo**
- **vpc end points needs enable dns hostname and dns support flags set to true.**
- we need to check the region of vpc end point created. Our request will work only if requestor is in same region or specify the same region in requests. 

- flow logs capture info in vpc. **VPC flow log, subnet flow log and ENI flow log**. VPC flow log includes other two. Flow log can go to s3 and cloudwatch
- flow log syntax: < version> < account-id> < interface-id> < srcaddr> < dstaddr> < srcport>
< dstport> < protocol> < packets> < bytes> < start> < end> < action> < logstatus>
- creating flow logs, we can choose to log all, only accept or only reject requests.
- we can use CW insights or athena over s3 to get more details from flow logs.
  
- baston host is used to ssh to private instances. baston is in public netowkr. and then controls who can access to other hosts. So baston host we allow port 22 from only our ip and from there to other instances.
  
- to connecct to entrprise vpn via internet: we can set up a customer gateway(softwre or hardware) at enterprose network and vpn gateway(vpn concentrator) at aws. Then link them via a site-to-site vpn. To link either use static internet routabel ip or NAT ip of enterprise network. 
  
- direct connet is above via aws network and direct to enterprise. We set up vpc gateway on aws to connect to enterprise network. allows us to connect to public resources(s3) and private resources (ec2) over same connection. This has a dedicated network connection between each other physically. 
- Direct connect gateway allows connection of multiple vpc in even diff regions to enterprise network. so multiple vpc connection is the main use of this. So no need to do the above thing for every vpc. this doesnt peer vpc .

- **Egress only internet gateway : Works for only ipv6. It allows only outward conenction. Similar to NAT, bt NAT is for ipv4.** needed cause all ipv6 all public. so we set up egress so that our ipv6 cannot be reached from outside. Also needs route table changes.

### Other Services
- aws equivalent for code is codecommit. and use codebuild to build it. Codedeploy to deploy or use beanstalk. Orchestrate all these using code pipeline.
- CF is infrastructure as code.

- ecs is a container orchestration service aws.
- ecs core : run docker container in user provisioned ec2 instances. Fargate: container on aws provisioned(serverless). EKS : ecs on aws powered kubernetes
- ecs task container can have its own iam roles.
- ALB on ec2: we can run same app on same ec2 machine  with multiple instances. We then use port mapping to link all of them together.

- ecs config is at /etc/ecs/ecs.config. in them 4 imp properties EC2_CLUSTER (assign to cluser), ECS_ENGINE_AUTH_DATA (to pull images from private repo), ECS_AVAILABLE_LOGGING_DRIVERS (cloudwatch continer logging), ECS_ENABLE_TASK_IAM_ROLE= true(enable iam task for ecs)

- ECR: elastic continer registry. managed by aws, integrated with iam and ecs. Https and encrypted at rest.

- step function: way to orchestrate severless function flow . Represented as JSON state machines. Timout, scaling, sequence, parallel, error handling etc. Integrate with EC2, ECS, On premise server, api gateway .Max excution time of 1 year.  And can have human approval feature.

- Simple Workflow service(SWF):**co ordinate work among apps. Not serverlss , works in ec2.** has activity and definition step. recommened to use step fn .  Use this if you need external signal to process or child process to return value to parent. 

- EMR: elastic map reduce:  Helps create hadoop cluster.Made of ec2 instances. Support spark, hbase etc. Auto scaling and integrated with spot instances.

- aws GLUE: Fully managed ETL. Serverless pay as you go, provisions spark. Auto code generation.

- opsWorks : managed chef and puppet
- elastic transcoder: convert media files in s3 to other format. it has jobs, pipeline, preset (tempates) and notifications.scales, fully managed and pay for what you use.

- aws organizations: allows us to manage multiple aws accoutns under one root or main organizaion. we get consolidated billing so we can get aggregated usage and discounts. We can use an api to automate child account creation. 
- we can have OU(oraganization unit) and Service COntrol polices( SCP): we can nest OU and have diff aws account under each of them. And use SCP to control them Similar to IAM, it filters on top of IAM to work.

- aws workspaces :managed cloud desktop. On demand pay per use. Integrated with MS active directory.
- **appsync: store sync data across mobile and web apps in real time. It makes uses of graphQL.**INtegration with dynamo and lambda. Offline data sync (Replaces cognito sync)
- SSO:  integrated with MS active directory. Needs SAML 2.0 enabled.

### Pillars of Architecutre: Well architected framework
- Operational excellance, Security, Reliability, Performance Efficieny, Cost Optimization. We shouldnt trade off one for another. Each of them enhance or improve each other.

- aws well architected service: we createa a work load about our app with all details needed. then we start periodic reviews, that asks questions about above pillars. This helps to evaluate our app. The app generates a report, has milestones, risk categorization etc.

- Operation Excellance: Includes the ability to run and monitor systems to deliver business value
and to continually improve supporting processes and procedures. All operations needs to be code(including infra). 
automate doc generation, make frequent small and reversible changes ,refine operation procedures freq and make sure team is aware of it,  anticpiate failure and learn from all operational failures.

- Security: Includes the ability to protect information, systems, and assets while delivering
business value through risk assessments and mitigation strategies. Strong identity management, enable traceability, apply security at every level, automate security best practices, protect data in transit and at rest , keep people away from data ( very limited access), prepare for security events( run simulations for recovery and actions)

- Reliability: Ability of a system to recover from infrastructure or service disruptions,dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.
Test recovery procedure, auto recover from failures, scale horizontaly to increase availabilty, stop guessing capacity requirements, use automation to chagne infra etc.

- Performance Effiiency: Includes the ability to use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve. Democratice advanced technologies( start usign them to help us work faster and reliable), go global in mins (deploy globally), use serverless architecture, experiment more often, mechnanical sympathy( be aware of all aws services)

- Cost optimization: Includes the ability to run systems to deliver business value at the lowest price point. Adopt a consumption mode( pay for what we use, ) measure overall efficiency( using cloudwatch), stop spendig money on data center opertions (aws does infra part for us), analyze and attribute expenditure(make sure to use tags etc), use managed and application level services to reduce cost of ownership

- AWS trusted advisor: analyze our aws account and provide recommendations on cost optimization, performance, security, fault tolerance, service limits . its not free, needs busienss plan to get some options.

- https://aws.amazon.com/architecture/

- Disaster recovery: Recovery Point Objective (RPO) and Recovery Time Objective (RTO)
- RPO: how often we run backups so that how far or which point we can recover to in a disaster.  So how much of a data loss we are willing to accept
- RTO: amount of downtime we are willing to accept in case of disaster.
- Strategies: Backup and Restore,  Pilot Light, Warm Standby and Hot site/multi site
- in case of rto: backup and restore > pilot > warm > hot. So hot site is the fastest and has shortest RTOs

- Backup and Restore: Has high RPO. Like this approach using ebs backup or snowball has usually a long time to back up ( few hours) so high RPO , also has high RTO. We do this becasue this is cheap.
- pilot: A small app (critical core) is always running in cloud. TO recover we just add the other parts to it. Faster backup and restore as critical apps are always running. So lower RTO and RPO. and low cost. Prefered for critical core systems.
- Warm:  Full system running but at minimum level. We can sacle it when disaster strikes as required. This is more costly. But smaller rpo and rto
- hot/multi site: very small rto in seconds. Full scale app running. Very costly.
