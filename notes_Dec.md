- And so there's a naming convention for the regions and usually it can be us-east-1, eu-west-3 etc. a region is a cluster of data centers. So each region can have many availability zones, So usually, it's going to be three AZ per region,sometimes you will see two and sometimes you will see six. So the regions and with a number and the AZ or availability zones, and with a letter abcdef.

-Each AZ is one or more discrete data centers,so imagine a lot of computers into one big room that's a data center.
And each data center will have redundant power,networking and connectivity.But all these AZ or availability zones are separate from each other, and that's why they're isolated from disasters. But even though they're geographically distant, and isolated from disasters,they're still connected with one another with high bandwidth, ultra-low latency networking.

- Resources created in regional services are  not visible across regions, while global services like iam are.

- IAM has users, groups and roles. Identity federation uses SAML standard.

- EC2 instance connect wont work if SSH port is not enabled.

- SG is locked down to region/VPC combo. SG issues almost always causes the timeouts.

- ec2 user data script is run only once, when instance first starts and never again. Its automatically run with the sudo command.

- resvered ec2 is for min 1 year. Dedicated instances are for us only but multiple dedicated instances can run in same host, while dedicated host is where the hosting system is for us. 

- on demand ec2, billing is per second after first min.Has highest cost of all types.Reserved is upto 75% cheaper, but pay upfront. Convertitle reserved is upto 54% cheaper. Spot are upto 90% cheaper. Dedicated host is for 3 year reservation.

- for spot, if price is over our bid, we have 2 mins to stop or terminate. We can have repeated spot req so that when our instancs are terminated, our spot req becomes active again and we might get another instance. We can cancel spot requests that are open, active or disabled.  Also cancelling a spot request does not terminate running instances for that request.  So first cancel spot req and then go and terminate instances.

- spot fleet: spot instances and optional on demand. This will try to meet our set targets like lowestprice, diversified instance types or capacity optimized. It will lauch instances until this target is reached adn then stop.Spot fleet allocates instances with lowest price automatically.  Spot fleet has lauch pools to choose instances from and we can define multiple launch pools as well.

- burstable instances (t2/t3) lose burst credits once they use it. So it goes back to non burstable type. We can pay money and get unlimited bursts.

- AMI locked per account/region

- cluster in same az and same rack or hardware, so low latency, but single failure. Spread, diff hardware diff AZ, limited to max 7 within one AZ per placement grp, low failure risk.. Partition, same az but diff partitions( diff set of racks). can scale to 100 of instances in partition, but max 7 partitions in AZ.

- ENI. logical component in a vpc that represents a virtual network card. Eni can have  multiple private ips,one public ip, elastic ip, SG, mac address. We can create eni and attach to ec2. There are in an AZ only.One ec2 can have many eni attached to it.Can be moved even when ec2 running. SG is attached to instance via eni.

- Each ENI lives within a particular subnet of the VPC (and hence within a particular Availability Zone)  and has 
Description,Private IP Address,Elastic IP Address,MAC Address,Security Group(s),Source/Destination Check Flag,Delete on Termination Flag.  single EC2 instance can now be attached to two ENIs, each one on a distinct subnet. The ENI (not the instance) is now associated with a subnet.re

- Similar to an EBS volume, ENIs have a lifetime that is independent of any particular EC2 instance. They are also truly elastic. You can create them ahead of time, and then associate one or two of them with an instance at launch time. You can also attach an ENI to an instance while it is running

- ec2 hibernate. fast start up. Ram is written to underlying ebs, but ebs needs to be encrypted. ram must be less than 150gb. available for on demand and reserved.cannot be hibernated for more than 60 days.

## Load Balancing

- classic load balancer ( v1, old version) http, https,tcp; application LB (v2 new version) http, https, websocket; netwrk LB (v2, new version) TCP, TLS and UDP. We can set up an internal to aws lb or an external LB. ELB access log will give all access logs and CW metrics the stats.

- CLB, health checks are tcp or http. fixed host name, multi az also possible.

- ALB is layer 7 only . can target multiple http endpoints across machines( but fixed hostname).machines are grped as target groups. They can be diff AZ. Useful for containers. Supports redirection from http to https. It supports routing to target grps , routing based on url to diff target grps.Also routing based on hostname. based on query string or headers. Has port mapping feature to redirect to dynamic port. We can have a set of ec2 or users and another for db and put them in diff target grps. Then ALB can route to them. ALB can also route to lambda fns. It can be also to private ip address. True ip hidden and set in x-forwarded-for and port in x-forwarded-port and x-forwarded-proto.

- NLB level 4, can handle milions of records, lowest latency. NLB has one static ip per AZ and supports assigning elastic ip(whitelisting ip).NLB not in free tier. IT has same target grp features like ALB. NLB the traffic to instances goes like coming from outside. So SG for instances needs to allow TCP from outside for health checks to pass and NLB to work. We dont add a SG to NLB unlike ALB. Use NLB for extreme perf or static private ip or elastic ip to NLB.

- Stickiness is for CLB and ALB. Works till cookie for stickness is not expired.

- Cross zone LB, distributes evenly across AZ. CLB disbaled by default, ALB enabled and cannot be turned off. NLB disbaled, charged for enabling. CLB And ALB is free.

- SNI solves problem of loading multiple ssl certs in one server. Client needs to indicate which hsotname to reach and then we can load that certificate. Useful for load balancers with tls. It only works for ALB and NLB and cloudfront.  CLB doenst have sni, so need to use multiple CLB for multiple tls end points

- Connection draining in CLB and Deregistration delay in ALB and NLB. Its time to complete in flight requests before deregistered by LB. Between 1 to 3600 sec, def is 300. Can be disabled as well.

## Auto Scaling Groups
- ASG will register new instances to LB
- ASG has launch config ( or newer lauch templates), which has instance type, user data, ssh keys, SG, EBS volumes. Min/max size, network/subnet, LB details, scaling policies. We can scale based on CW. IAM roles attached to ASG will get attached to instances.

- we create lauch template and then attach it to an ASG. And in ASG we say instances required. min max, LB etc

- scaling polcies: Target tracking scaling- like avg cpu with asg need to be within 40 and asg works based on that. Simple scaling and Step scaling - set CW alarms and set rule like on that alarm to add 2 and another CW alarm for lower values to reduce instances. Also Scheduled Scaling actions - if we knwo the load before hand.

- Scaling cool down: Ensure no instances are launched or killed before prev scaling is done. We can also create specific cooldown for simple and step scaling policies, which overrides the default one.Default cool down is 300sec.

- ASG def termination polciy: Fnid one with max instnace and delete one with oldest config

- Instance launch -> pending state ( pending hooks here pending:Wait and pending:proceed) -> In Service -> Terminating (hooks terminate:Wait termiante:procced) -> terminated

- Lauch template vs config: config is legacy, and must be recreated every time we need to update. Tempalte is newer and recommended.Template can have variables and reusable.

## EBS

- its a network drive. can be detached from one ec2 to another if same AZ. So locked to one AZ. Has provisioned GB and IOPS. We pay for provisioned and not used capacity. We can increase capacity as we use it. 

- 4 types: GP2(SSD)-general ; IO1(SSD)-high perf low latency high throughput; ST1(HDD)- low cost high volume throughput intensive tasks freq accessed tasks; SC1(HDD)- low cost for less freq accessed

- Only gp2 and ioi as boot volumes. Add entry to /etc/ftab to mount an ebs on every restart. New ebs might need file system created and then mounted to ec2 to use it

- GP2: 1gb to 16tb. Burst to 3000 and max of 16k iops. We get 3iops per gb. So at 5443gb we are at limit. So if we incease size after this, iops wont increase. Since burst is 3000, its available for size < 1000gb.

- IO1: 4gb to 16tb.it has provisoned iops. 100 to 64k( for nitro), else 32k. Max ratio of gb to iops is 1:50. So for each gb we get a max of 50 iops.Useful for db

- ST1: useful for strreaming, big data etc. 500gb to 16tb.max iops 500 and max throughput 500Mbs and burstable.

- SC1: 500gb to 16tb. max 250 iops. max 250 throughput and burstable

- ebs snapshot is incremental. they use io so run it when load is less.stored in s3 and no need to detch(thogh recommended). max 1lac snapshots. copy btwn az and create ami. Amazon data life cycle manager can automate this.

- with ebs encryption: all data at rest ,all data in transit, all snapshot and all volumes created from it will be encrypted. to encrypt an unencrpyted one, create snapshot, encrypt it, create new ebs volume from that.

- instance store cannot survice stops or terminations, but can survive restarts. Cannot resize on fly or take automated backups. they can have pretty high IOPS as they are physically attached to ec2. Its a block store like ebs. 

- ebs is alraedy multi az. RAID 0 and 1 are possible while 5 and 6 are not recommended for ebs.

- raid 0, performance :data goes to either volume 1 or 2. total size is 1+2. Perf increases and faster retrieval, but lose data if one volume fails. It increases iops as its faster. So we can combine two ebs into raid 0 and get double iops. 

- raid 1 is for fault tolerance. Low perf but increased reliability. It inceases network usagae as we need twice network throughput for same data.

## EFS
- managed network file system. It can be mounted across multiple az, while ebs was locked to be used in a single az ( though its backed up in multi az ,we cannot use it at once in multiple az, needed to copy and use it). But efs can be used.So highly available , but costly and we only pay for what we use. uses nfs4.1 and need a SG to control acess( network security). only works with linux ami (windows cannot mount it). it has auto backup

- Encrypt at rest using KMS. No need for capacity planning. Grows automatically in size. We can set it to General purpose ( default, low latency for web servers, cms) high iops( high latency, but high throughpuot as well for big data, streaming)

- has storage tiers and we can set rules for auto move by life cycle management. Standard ( for freq access), in frequent tier( for less accessed files, low price but price to retieve.)

- efs throughput can be bursting( increases with file system size) or provisioned ( remains same)

- sg for nfs needs to allow NFS traffic. Usually we set allow from SG of ec2 using that efs.

## RDS
- rds is managed db service. aws has postgre, oracle, mysql, mssql, mariadb and aws aurora.
- managed ones have automatic provisioning, os patching, continous backup and restore, dashboards, read replicas and multi az.Also vertical and horizontal scaling and maintanence windows. Storage is by EBS. But we cannnot ssh to the instances running these managed instances.

- auto backup. daily full bacup and txn logs every 5 mins. so we can restore upto 5 mins back in time and to the oldest from there. Backup held for 7 days by default and can be increased to 35 .Snapshots are alos possible , manually triggerd by users.

- upto 5 read replicas. in same az, cross az or cross region. replication is async, so reads are eventually consistent. we can promote read replicas to db. Apps must update conn string to use read replicas.

- if read replicas is in diff az, then replication incurs cost for data movement across az. we can so put replicas in same az wherein the replication is free. 

- multi az is sync replication. we have one dns for all multi az. We have auto failover. failover if we lose entire az, network, storage or instance. its automatic. ( slave master concept). 

- we can set up read replicas as multi az. 

- aws rds has iam authentication. For mysql and postgresql.

- we have at rest encryption, and is set only when db is created.  we can encrypt master and read replicas. if master is not encrypted, read replicas cannot be encrypted as well. Transparent Data Encryption(TDE) available for Oralce and SQL server. There is also inflight encryption, using SSL certificates. To force this, in postgres sql set rds.force_ssl = 1 and in mysql, use grant to force ssl.

- to encypt an unencrypted db, take snapshot, copy it , enable encryption in that copy and restore db from that. Then migrate the apps to new one.

- aurora is aws propietary. both postgre and mysql suppored and needs to choose one at creation. ( so apps can use drivers of either to connect). Its cloud optimzed and has 5x perf over mysql and 3x over postgre. It can have upto 15 replicas. failover is instantaneous and has high availablity.

- it has 6 copies acorss 3 az.Need 4 out of 6 for writes and 3 out of 6 for reads.Its self healing and has peer -peer replication. It has a master to take writes and in failover new master is selected in 30 secs. Any of the 15 read replicas can become master. SUpports cross region replication. 

- aurora has a reader end point which does load balancing over all teh read replicas. it has only one master end point to connect to for writes, which never changes. Also we can have multi write aurora is needed. 

- we can restore to any point of time in aurora. zero down time patching as well.

- there is aurora serverless . good for infrequent, intermittent or unpredicatable work loads. No need to do capacity planning. pay per second as well.

- global aurora. One way for gloabl is with cross region replicas. But aurora gloabl is spread across regions. 1 primary region for read and write. Upto 5 secondary read only regions, with less than 1 sec lag. Upto 16 read replicas per secondary regions.We have promote secondary to primary region in less than 1 min.

- elasticcache has multi az failover for redis and read replicas.Also redis has auto backup and restore. Memcached is more towards sharding.Redis has encryption at rest(kms) and in transit( using redis auth)

- no elasticache has iam auth. redis has redis auth for this. Memcached supports SASL auth.

- in cache, lazyload: store all read data but data can be state , writethrough: update cache on write so no state, session store: store session details

- postgre port 5432, mysql 3306, oracle 1521, maria 3306, mssql 1433. Aurora either 5432 or 3306 depending on choice.

## Route 53

- alias is a host name to an aws resource. cname is hostname to hostname , A record for ipv4 and AAAA for ipv6
- it has load balancing throug DNS(client load balancing), health checks and routing policies.
- 0.50$ per hosted zone

- load balancers are regional. So cannot balance across regions. Use route 53 to redirect traffic to diff regions.

- aws resources expose their own hostname which are quite long usually. Using cname we can map this to a more user friendly hostname. But cname can map a hostname to only a non root host name. Alias name map a hostname to an aws resource like s3.amazonaws.com. It can map root names as well. Alias is free and has health checks as well.

- simple routing: redirect to single record. No health checks.If we return multiple values, client will randomly choose one entry. 

- weighted routing: percentage of req to a resource. Some of weights dont need to be 100. AWS calcuates avg and finds the route. can have health checks

- latency routing: route to lowest latency route. So if user in germany can be directed to us if that region has low latency. 

- route53 can do health check and stop sending traffic to it. Deemed unhealthy or healthy based on 3 checks in row.default interval for check is 30sec. can be set to 10 for higher cost. have  http, https and tcl checks. but no ssl verification.health checks can be linked to route53 queries

- failover routing: it needs mandatory health checks. We create two entries in route53 for this. one primary and one secondary. Primary needs health check while secondary may not have it.

- geolocation: routes based on user location. we say traffic from a loc to a certain route. Also set a default entry for loc that doesnt match.Loc can be continent or country.

- multi value: route to multiple values. Return upto 8 values. Its not an substitue for elb.Each entry has a health check. So route 53 wont send records that are nt healthy.

- route53 is also a registrar. for a third party domain name, make a hosted zone in route53, update NS records on third party to use route53 NS . we get NS records on creating hosted zone.

## Beanstalk
- golden ami. one with all req apps installed and ami created out of that. for speed create db and ebs from snapshots for quick bootstrapping

- it is a managed stack. it has single instance deployment, LB+ASG ( for webapp), ASG only( non web).

- it has three parts. Application, application version and environment name. We can deploy to an env and then promote same app to another env.has rollback and full control over deployment.


## S3

- s3 is file based. each bucket is gloably unique named. buckets are created regionally though. Names should be lowercase, no underscore, no ip, between 3 and 63 chars and start with lower case letter or a number.

- key to a file is s3://bucket-name/file-name. Also like s3://bucket-name/dir1/dir2/myfile. Here dir1/dir2 is prefix and myfile is filename. There is no physical directories in s3, just nested keys.

- max object size is 5TB. but max upload is 5GB. so for larger size do multi part upload. each file has metadata and tags.Also versioning.

- s3 is global (hence name req) but buckets are in a region.

- version is enabled at bucket level. If we reupload, new version of file is visible, old versions hidden. prior to enable versioning, version is null.so if we upload file then enable versioning then upload again, new file will have a new version and old one still have null as version.  If we suspend versioning, old versions are not deleted. Delete a versionined file adds a Delete marker to latest version of that file. So its just hidden by delete marker. (all prev verions remains as well). If we permanently delete, we need to delete the file with delete marker set and all versions.  Now if we delete delete marker file and all prev versions but one then file becomes visibel again with taht version.

- there are 4 s3 encrptions:
sse-s3:(server side enc, keys managed by s3): must set x-amz-server-side-encryption=aes256.
sse-kms(as as above, but key managed by kms): Gives control over keys and audit for key. set x-amz-server-side-encryption=aws-kms
sse-c(client provided key): https must be  used and key provided in header.
client side encryption: client side. 

- s3 encryption is for a specific file and version if we do it while uploading. Not for entire bucket.so same bucket can have encry and not encry files.If we want at bucket level, we need to edit bucket settings and enabel encryp.

- s3 exposes a http endpoint where no encry in transit and https endpoint where we have encry in transit

- s3 has iam based access using policies(user based). Also has resource policies at bucket level, allowing cross acount access(resource based). it also has Object access control and bucket access control(less commonly used now). An IAM can access s3 bucket if iam policy or resource policy allows. There is no explicit deny

- s3 policy has these parts:version, sid, effect(allow,deny), principal(whom), action(which s3 operation, like putobject), and resource(s3 bucket arn). using this we can disable public access to bucket. 

- s3 has vpc endpoints. logging audit in another s3 bucket.API calls to cloudtrail. MFA delete in versioned buckets.pre signed url for limited time access.

- s3 website address is bucketname.s3-website.aws-region.amazonaws.com or bucketnames3-website-aws-region.amazonaws.com

- s3 has read after write consistency for put of new objects.Eventual consistency on delete and put of existing objects. there is no way for strong consistency

## AWS CLI

- ec2 metadata from http://169.254.169.254/latest/meta-data/. We can get iam role from this, but not the policy.

## Advanced S3

- can enable mfa for permanent delete of obect, suspend versioning. Not needed for enable versioninig.only root owner can enable/disable mfa. mfa can be enabled by using only aws cli.

- access logs can be sent to another s3 bucket.if both buckets are same then it will grow exponentially large.

- versioning must be enabled for replication. we have Cross region repl and Same region repl. Buckets can be in diff account. copy is async. After enabling repl, only new objects are replicated. Not retrospective. Any delete (even versions) are not replicated. there is no repl chaining. 1 repl to 2 and 2 to 3, but objects in 1 wil nt repl to 3.

- s3 standard, standard-IA, one zone-IA, intelligent tier, glacier, and glacier deep dive. (reduced redeundancy is deprecated)

- s3 standard: 11 9 durability. 99.99% avaialility. two az fail survive.
- s3 standard-IA: same durabilty, 99.9% availability. Less freq accessed data, but rapid retriveal when needed. Lower cost compared to standard s3. Also sustain 2 az fail.
- s3 1z-IA: singe az.same durability, but one az loss leads to loss. 99.5% availability. low latency and high throughput. lower cost and supports ssl .
- s3 intelligent tiering: same durability as standard. small fee for monitoring and movement. will auto move objects based on access patterns. can surive 1az failover. 99.9% availability
- glacier: low cost for long storage. 11 9 durabilty. it has retrieval cost. each item in galcier is an archive (40tb) and archives are stored in vaults.it has expedieted ( 1to 5 mins) retireval, standard ( 3 to 5 hrs), bult ( 5 to 12 hrs). Min storage duration is 90 days. 
- glacier deep archive: retrieve is only standard( 12 hrs) and bulk (48 hrs). min of 180 days storage.

- retreival cost is per gb retrieved for whereever applicable.

- standard IA and 1z IA has 128kb as min size, while galcier and deep galcier has 40kb. others no min size.

- s3 intelligent, standard IA and 1z IA has 30 days min storage.

- in any tier if we try to move object before min days required , we need to pay money.

- glacier, deep, standard and standard IA has 3 az availability. Rest 1 az.

- we cannot move object from glacier to IA. We need to restore object and then copy it to IA

- moving objects betwen tiers can be automated with life cycle rules. Rules can have many configs, like only for specific prefix etc. Using rules we can move prev version or objects, curr version of objects or expire objects etc.

- glacier expedieted is 10$ for retrieval. galcier retireval cost is more than all other options.

- Using s3 we can get 3500 puts/delete and 5000 gets per second per prefix per  bucket. There are no limits to prefix in a bucket

- if you use kms with s3, we may be limited by kms limits( 10k or 30k or 5k based on region) and we cannot ask for increase in kms quota

- multipart upload recommended for >100mb and must for >5gb. Also use transfer accelaraion by using edge locs.
- also speed by downloads by requesting a specific byte range and can be used to get a file in parallel.

- s3 select and glacier select to query data. only select and no joins. Less network usage for analysis as the data is queried and only result is returend. so around 400% faster and 80% cheaper.

- events are sent for every write if version enabled. If no version and two writes at once, only one event will be sent.

- athena is a serverless analytics service on s3.sql lang is supported. has jdbc/odbc drivers. Charged per query and data scanned.supports a lot of fomrats including parquet.athena has groupby , aggregation join etc.

-s3 object lock: write once and read many, block object version delete for a specified time, prevent modification. Similar one for glacier lock.

### CloudFront

- CFrnt gives DDOS protection, integartion with AWS Shield, AWS webapp app firewall. can expose extenal https endpoints and internal https endpoints

- Cfrnt origin can be from S3. Protected by Cfrnt Origin Access Identity(OAI). CFrnt can also be used as ingress to add files to S3. Or origin can be any http endpint, like alb, ec2(sg must also all public ip of edge locations), s3 website, custom end point etc. ec2 and alb must be public to be used with cfrnt.

- Using OAI and s3, we can restrict s3 access only via Cfrnt and not directly

- Cfrnt has geo whitelisting and black listing.

- Cfrnt is gloabl network, while s3 must be set up in each region requried. Cfrnt great for static content while S3 for dynamic content.

- we can create signed url in cfrnt. that gives access to certain ip for certain time. and signed providers. We can crate an url for each file or a signed cookie that gives acces for entire duration its valid.

- Cfrnt signed is account wide, only root can provide it and filter by ip, path, date ,expiration etc and origin can be many sources. S3 signed is from an IAM user

- global accelator wrks with elastic ip , private or public ec2, nlb and alb.It creates two anycast ip for our app. Has health checks as well. DDOS protection from aws shield

- Cfrnt is good is dynamic content. Global acc for non http use cases, http with static ip etc.

- CloudFront uses Edge Locations to cache content while Global Accelerator uses Edge Locations to find an optimal pathway to the nearest regional endpoint.CloudFront is designed to handle HTTP protocol meanwhile Global Accelerator is best used for both HTTP and non-HTTP protocols such as TCP and UDP. 

## Storage in AWS

- snowball a physicall box to transfer data.Its ecnrypted, sns tracking and eink label. 
- snowball edge: 100tb capacity, storage optimzed (24vcpu) or compute optimzed(52 vcpu).Has computational capability.Supports custom ec2 and lambda.
- snowmobile: 100peta bytes 
- snowball cannot put to glacier directly. We need to copy to s3 and move to glacier using life cycle props.

## Storage gateway

- aws has block storage( ebs, instance store), file(efs) and object (s3, glacier). Storage gatway links this with our own premise storage creating a hybrid cloud.

- three types: File , volume and tape gateway.

- file: configured s3 bucekts using nfs and smb protocol. s3, s3IA and s31zIA. most recent data cached by file gateway. can be mounted on many servers.File gateway is created on premise and linked to aws network. So local apps wil connect to file gatway and get data from aws, but it feels like we are getting data from local network.

- volume: block storage. Using iSCSI protocal backed by s3. For eg. ebs snapshots can be used to restore onpremise instances using this. cached vol gateway : gives low latency most recent data , stored volumes: entire data set backed to s3 periodically.

- tape: backed  up in tapes. So same process to copy to tape, but in cloud.Virtual Tape Library (VTL) backed up s3 and glacier.back up data using existing tape based process and iSCSI interface.

- usually storage gateway needs a virtual device to interface. now we can buy File gateway hardware appliance to interface frmo amazon.

## FSx

- EFS is for posix (linux) system. FSx for windows is a fully mnanaged windows file system share drive. Supports SMB and windows NFS. Supports Active directory, ACL and user quotas.Built on SSD and quite high scalable. can be multi az and backed up to s3.

- Lustre is  a type of parallel distributed file system for large scale computing and high performance computing. Scale quite well, seamlesss integrated with s3. can read frmo s3 and write to s3 through FSx.Can be  used from on premise servers.

## SQS, SNS, Kinesis, MQ

- sqs has unlimted througput and no of message. default message stays in que for 4 days and max 14 days. Message needs less than 256kb per mesage. at least once delivery, so duplciate message possible. Also best effort ordering so out of order messages. Consumers may receive upto 10 messages at a time. encrypt at rest using kms, in fight using https and client side encrypt, IAM based access policies.

- cannot change que type after creation.

- sqs visibility timeout: When polled message is invisible for a time period.So process message within this time. If need more time call changeMessageVisibility to get more time.

- We can set how many times a message goes back to queue. After MaximumReceives threshold is passed message is sent to dead letter queue. They also have expire of max 14 days.

- sqs delay queue: Delay is upto 15 mins.can set delay at queue level or per message delay. can override value using DelaySeconds parameter.

- sqs fifo: limited troughput. 300 msg/sec without batcing and 3000 msg/sec with batching( 10 at a time). Exact once delivery and in order delivery.

- SNS: one message to multiple, pub-subscribe.Upto 10lac subs per topic. subscribers can be sqs, https/http, lambda, email, sms ,mobile . Has at rest encry by kms, in flight encry and client side encryp. IAM policies and SNS access policies( similar to s3 bucket and sqs access policies), for cross account access.

- sns cannot send to sqs fifo.

- kinesis data is auto copied to 3 AZ. It has streams, analysis and firehouse to  load data.
- data retention by default 1 day to max 7 days. Can reprocess replay data.Data is immutable and once inserted cannot be deleted. 1mb/s write and 2mb/s read per shard.Billing is per shard provisioned. We can reshard later. Records are arranged per shard. 

- put record uses a key to find shard. If sendind too much data, then provisioned throughput exceed exceptions. Kinesis consumers use dynamodb to track.

- Iam policies access. Encry at rest, flight and client side. Also vpc endpoints.

- firehouse to load data to s3, elasticsearch, redshift, splunk. near real time, upto 60 sec latency for non batch data or min 32mb of data. pay for data through firehouse. provision throughput to be useful.

- in sqs fifo, to group messages, use a group id. else messages are consumed in order of creation

- amazon mq uses open sorce protocols. Its managed apache active mq. doesnt scale like sqs/sns.Runs on dedicated machines and can run in high availabilty mode for failovers. It has both queue and topic feature.

## Serverless

- fargate is serverless
- lambda env variables needs to be below 4kb. /tmp folder has 512 mb max.
- lambda@edge: we deploy lambda fns in CF distributions. Using it we can modify viewer request, origin req, origin response and viewer response.

- apigateway is in one region but can be edge optimized so that its faster.

- apigateway can verify iam creds. Its passed on header as sigv4. it can also use cognito or lambda authorizers.

- cognito user pools help sign in for app users.Identity pools are for federated  users. user pools creates a serverless db of users and can have federated identities.Cognito federated allows users to loginto aws directly from client network. Appsync is used now instead of cognito sync foy syncing user data, requires federated identity pool to work.N

## DynamoDB

- fully managed high available, no sql db. Serverless and by default 3 AZ. IAM for security authentication etc. Max item size is 400kb.0.00013 per read and 0.00065 per write.

- dax: dynamodb accelator. Seamless cache for dynamodb. 5min ttl by default.upto 10 nodes in cluster. 3AZ . Dynamodb also has streams for changes in data.

- txns: since 2018. upto 10 unique items or 4MB data. insert, udpate, delete

- On demand provisioninig. Dynamo has VPC end point. Encry at rest and transit. Point in time restore like rds and backups.

- aws DMS to migrate to dynamodb.

- global tables: Cross reg replicated. Needs streams enabled. 


## Databases

- must provision an ec2 for elasticache(memcached or redis)

- redshift is based on postgresql, its an olap. its columnar storage.it has massive parallel querying. can query using sql. BI tools such as aws quicksight or tableau integrate with it. 

- redshift has leader node and compute nodes. Compute nodes sends answers to leader node.

- redshift spectrum to perform queries directly against s3, without loading data. Also has redshift enhanced vpc routing. Has automated snapshots, incremental. It can be every 8hrs or every 5gb or on schedule.We can copy snapshots and move it as a new cluster to another region.

- neptune: fully managed graph database.3az, upto 15 read replicas, point in time recovery, continous backup to s3 etc.

- elastic search has built in integration for kinesis data firehouse, aws iot, CW logs. comes with ELK stack. its multi az,

## Monitoring

- CW dashboards are global. so single dashboard can have graphs from many regions 3 dashb upto 50 metrics free. 3$/month after that

- CW can get logs from Beanstalk, ecs, lambda, vpc flow, api gateway , cloud trails, cw agents like in ec2, route53
- can encryp logs using kms

- to push logs from ec2, run CW agent in ec2 with correct IAM permissions.CW logs agent is the old one, while CW unified agent is newer and can send additional logs(also centralized SSM config, send logs outside CW ).

- CW alarms can be in OK, in sufficent data, Alarm

- ec2 has instance recovery to recover instance (same private and public ip, same elastic ip, same meta data, smae placement grp etc). CW alarms can be used to do this.

- CloudTrail is enabeld by default.

- AWS config helps wth auditing and recording compliance of aws services over time.It records config and changes over time. We can then store in s3 and analyze. So we can check config history and see if ssh was enabled or not. We can receive sns alerts .Per region service but can be aggregated over all regions.

- we can set config rules. like check if all our ec2 instances are of type t2.micro or have a certain ami. Rules can be triggered and checked. Rules also have auto remediation settings. Config cannot prevent anythign from happenig, but once it happens it can take aciton. no free tier, $2 per active rule per region per month.

## Advanced IAM

- sts gives out temp tokens for access. It helps with assume role( with saml or aws or webidentity). THough cognito is preferred over webidentity. temp creds valid between 15 mins to 1 hr.

- identity federation allows users outside aws to get temp creds to access aws. It can be via saml, cognito, custom identity broker, sso, webidentity, non saml with microsoft ad.

- saml2.0 Federation: great with AD.Can use with any service supporting it. Here we auth with an external id provider. It valdites and returns a SAML assertion. We pass it to aws sts, which validates assertion with its already defined trust and then gives out access token.Same flow for consoel login. Here after assertion verify, aws will redirect our browser to console. So here external id provides can be AD(like in Nike). We need to enable trust between aws iam and saml. saml2.0 suports web based cross domain sso. In sts we use assumerolewithsaml. Though this isthe old way of doing it, Amazon SSO Federation is the current way.

- if our existing ID provider is  not saml compliant, then we need to create our own custom identity provider. Here identity provider must determine appropriate iam policy.In sts, AssumeRole or GetFederationToken. Here unlike saml, after auth, identity provider wont return an assertion to us to exchange with aws to get access. Instead custom identity provider will talk to aws sts, get token and pass it to us. We will then use it to get into aws.

- webidentityfed: not recommended, use cognito(allows anonymous users, mfa, data sync) . Here we login using an external app(like fb, google), then pass the auth details to aws sts, get access token and then use it to access aws. But this is not a recommended way.

- cognito: Helps to provide direct accss to aws resource from external login, like fb or google. Here we login to external provider, get a login token, then exchange that token with aws cognito federated. It will validate the token with external id provider and if fine, will give back access token. the access token comes with predefined iam roles to restrict access. 

- AD services: Aws managed md : create our own ad in aws, create users and manager with mfa support. We can create trust between aws ad and on premise ad and use them inter operably; ad connector: connect on premise ad, ad connector is a proxy that sends request to on premise ad, so here no users in aws; simple ad: this cannot be connected to on premise ad.

- Aws organizations: Its a global service.allows to manager multiple aws account. Main accoutn is called master and other accounts is called member accounts.Member accounts can only be part of one org. Consolidated biling and one payment.Pricing benefits for aggregated usage. There is an api to automate account creation. AWS has organization units (OU) for grouping teams. So root OU has root aws account, then child ou with in (like retail, store etc) and each of these ou has its own member aws accounts. So we use OU to organize our member accounts.

- we have service control policies(SCP) to whitelist or blacklist aws actions. Applied at our or account level, but not for master. But scp in a member account is applied to even the root user for that account. SCP wont affect service linked roles(which allows other aws services to integrate with aws orgs). SCP denies everything by default and must have explicit allow.

- SCP is inherited at OU levels. So usually we attach an allow all SCP to root. Now all child accounts gets this. We then attach scp to child accounts to deny services there. Now if a parent has a DenyA policy and its child ha AllowA, then Deny will work. Deny has precedance over allow and parent rools always wins.

- to move aws accounts between orgs: remove account from existin org, send invite frmo new org and accept and join tehre. To migrate root account, remove all member accounts under it, delete old org and then invite and move

- similar to SCP we have Tag Control Policies as well.

- IAM conditions: using it to restrict iam policies. awsSourceIp: restrict access to only certain ip, by using deny all and then notip, awsSourceip our required ip address. Another is awsRequestedRegion. Also we can use StringEquals in IAM policies to compare tags and then allow/deny. We can force MFA by checking if mfa flag is true.

- IAM for S3: For bucket level permission( like listbucket) arn ends with bucketname. But for object level permissions (get,put etc) arn needs to include all objects within bucket so needs to end with /*

- iam role vs resource role: Iam role using which we can switch role while resource polciy is a policy of a resrouce .(like s3 resorce policy). So in iam case, we use switch role and get permissions of new role, giving up permissions of current role. But for second case, we just use resource policy while maintaining our own role.

- iam permission boundaries are for users and roles( not for groups). advanced policy to set max permission an iam entity can get. IAM bounday is overall things an user can do( like s3:*, iam:* etc) and iam permission is what we can do within this bounday (s3:put). If iam permission is outside iam boundary, then we get no permissions. So if we attach an admin policy to an user, but set his permission boundary to just s3, he can only access s3 even though with admin policy attached.

- permission boundaries can be used with aws orgs with scp. So here his effective permission is combination of org scp, permission boundary , and polcies for him.

- iam eval : org -> resource -> boundary -> session(sts) -> identity based policies.

- if an iam policy has deny:* and allow:s3put, then all s3 action inclduing s3 put are denied. Cause iam deny takes precedance always. Also we are denied everything that is not described in iam policy as well (by default everything is denied)

- aws resource access manager(RAM): allows aws resource we own with other accounts. This helps avoid duplicates. We can share vpc subnets from same org. Currently we cannot share SG and default vpc. If we share vpc, others can use vpc anc create resources in it. Bt they cannot modify the vpc or view resources in it. So we create a vpc, create subnets and share subnet .Now diff acounts gets subnets in same vpc, so can talk to each other. We can also enforce vpc level sg and rules. 

- aws SSO: its integrated with aws orgs. So one login for entire org accounts.Supports saml2.0, AD. centralized permission and auditing.

## Security

- kms is fully integrated with iam. It has two types of keys. Symetric(aes-256): single key for encryp and decryp, aws services integrated with kms use this, and its needed for envelope encryp. also we never get teh key for ourselves unencrypted and needs to use sdk. Asymetric key( rsa and ecc keys): public and private keys, used for sign/verify. can download public key but never get private key unecrypted.

- kms: Three types  of customer managed keys: aws managed service defualt (free), user key(1$ per mnth) and user imported (1$ per month). KMS costs 0.03$ per 10000 calls. kms can encrypt upto 4kb in a single call. if data >4b use envelope encrypt. To grant access to user check that key policy allows user and iam allows api calls. 

- kms are region specific in aws. Now to transfer an encrypted ebs volume, create snapshot and then copy to anothr region. aws will decrpyt data from one region and we can giv another key using whcih aws will encrypt ebs copied.

- cloudHSM: Hardware Security Module. aws provides the encryption hardware but we have to encrpy the data. Even encryp keys are to be managed by us.HSM is tamper resistant. CloudHSM is mutli az but we need to enable it. It supports both symettric and asymetric keys. No free tier. Redshit has integration with this. Now here aws cannot recover keys if we lose our encry keys. Aws just gives a hardware and rest is with us. (compliance)

- aws shield: to protect from DDOS.Shied standard is enabled for every aws accoutn and protects from ddos and other layer 3 and 4 attacks. Shield advanced gives ddos mitigation for $3000 per month per org and protects against more sofiscated attacks and 24/7 ddos support

- WAF: web applciation firewall. protects from layer 7 web attacks . deployed i ALB, api gateway, Cloudfront etc. We define web acces list (ip, http headers, body, uri params etc). Also frmo sql injectin, cross site attacks. And can protect for geo restrctions, size constraint for query. Also rate based control for ddos.

- AWS firewall  manager: manage rules of all WAF in an org.

- shared responsility: 

## SSM parameter store

- store config and secrets, optionally encry with kms. its serverlss and scalable.It has version tracking. It has CW events integration, CW logs, CF etc. We can store config in a tree style heirarchy with paths.

- std tier  has 10k params per accoutn, free, max 4kb param size, no parameter policies and free standard through put and pay 0.05 per 10k for higher throughput. Advanced is just oppisite, 1L per account, 8kb size, policies, 0.05$ per param per month and pay for standard and advacned throughput

- using param polcies we can set expire ttl and force update values. Can set multiple policies against one param aswell.

## Secrets Manager

- compared to ssm parameter store, secrets manager can rotate secrets every x days. Also we can create new secrets on rotation using aws lambda. Its integrated with rds to sync secrets between databases and secret managers. So it means both secrets manager and rds get updted at same time. This is more for rds, but we can use it to store our own secrets as well.

## VPC

- default vpc has internet conn and public ip for all instances. We also get public and private dns

- max 5 vpc per region, though can be increased on request. Each vpc can have upto 5 cidr and each one has min /28 and max /16 ip. Private vpc has 10.x.x.x , 172.16.x.x, 192.168.x.x cidr

- aws reservers 5 ip in each subnet. first 4 and last one. Last one is the network broadcast adderss. FIrst is the network address, next vpc router, next for mapping aws dns and next for future.

- in subnet config, set auto asign public ip to true if you need public ip for instances.

- Internet Gateway, IG, scales horizontaly, is HA and redundant. One vpc can be attached to only one IG and vice versa . IG is a NAT for instances with public ip in your subnet. Attaching IG to vpc wont allow internet access, need correct routes addded as well( this comes added in default subnet)

- NAT must be in public subnet. It must have source dest check turned off. And needs an elastic  ip attached to it.

- NAT gatewy is created in a specific AZ. It cannot be used by an instance in same subnet as it is.5Gbs and auto scale upto 45Gbps. Its resiliant in a single AZ, neeed to create multple nat gatway in multi az to make it multi az fail safe. We can do like two private subnet in two az, one NAT gateway and set routes, but then if NAT gateway fails both subnets fails. So create separate NAT gateway in each AZ for resiliance.

- enableDNSSupport: default true, and if true aws dns server (169.254.169.253) is queried. enableDNSHostname: default true in default vpc and false in any new vpc. If this and other flag is true then aws will assign public dns name.

- set both to true if we have a private zone in route53, to get access. Private hosted zone in route53, we can only access within vpc,and route53 is resolved within that. so we can have an entry like facebook.com point to google.com as cname here.

- any inbound request is via NACL -> SG -> instance. SG is stateful( so if inbound is true, outbound is true as well). NACL is stateless. Default NACL allows all inbound and outbound. One NACL per subnet, new subnet by default get default NACL. NACL rules have higher precedance with lower rule nos. ( rule nos  from 1 to 32766). In new NACL everything is denied . NACL is great in denying specific ip conn

- in a NACL if we remove all outbound rules, we can stil get output frmo server cause if SG allows outbound port. (!!!). If we do deny all in outbound NACL, we will not get any response.

- ephemeral ports (1024-65k) must be opnened in NACL. reasn is diff servers, ports, os use diff ports and it  changes. So better open it all and then deny the ports specifically. those which we dont need.

- VPC peering connects two vpc privately using aws. Needs non overlapping cidr.SO these two behave as its same network. This is not transitive. We can peer with another account. Route tables in both accounts needs to be udpated for this. So peering can work cross region and cross acount. 

- We can refernce SG of a peered vpc. after creating peering, update route tables to point to cidr of peered vpc for those requests. same as we do with subnets and IG.

- vpc end points allows connections to aws services, from our private subnets via aws network . We need to set up this. they are scalble horizontally and redundant. They remove need fot IG, NAT etc. there are two types: Interface:provisions an ENI and we can attach it, must have SG and most aws services use this. Gateway: provisions a target and must be added to route table (s3 and dynamo). 

- while creating vpc endpoint, we need to select a regin where service is. Now later when we query vpc endpoint if we use a diff region, then vpc endpoint wont be able to conncet to the service as it was configured for a diff region. So always give correct region.

- flow logs capture information about ip traffic in vpc. We have vpc flow logs, subnet flow logs and ENI flow logs. it can be send to s3 or CW. it can capture network info from our service and aws interfaces like elb rds, elasticache instances , redshift, workspaces etc

- use athena to query vpc flow logs from s3.

- site to site VPN: we need to create a customer gateway and aws creates a vpn gateway.then we create a site to site vpn connection between them, connecting them so that we can talk to our corp network from aws. Here vpn gateway is at vpc level. customer gateway can be software or hardware. We need the internet routable address of our customer gateway or public ip of NAT at our end to set site to site vpn

- direct connect: The above one is via internet. DC is private conn between aws and our network.  We still need to set up a VPC gateway for our vpc. We can access both public and private resources via DC. If we want to set up DC to many vpc, we need to use a DC gateway.

- DC connections: Dedicated (1Gbps or 10Gbps). Hosted (50Mbs, 500Mbs, 1Gbs). Dedicated is via physical connection 

- DC data in transit is not encrypted. If we need encryp, use AWS DC+VPN to get ipsec encryp

- Egress only ineternet gateway: works only for ipv6. Same as nat but for ipv6. All ipv6 are public. So egress allows us to set up exclusion for outside connection. Need to set up route tables here.

- AWS private link (VPC endpoint services): if we want to expose a service to many vpc, we either neeed to make it public or make it via vpc peering(which exposes whole network).better way is to create private link. Its scalable and secure.It doesnt need IG, route table or NAT. needs an NLB at aws and ENI at customer VPC. the NLB and ENI are pivately linked

- aws classic link and ec2 classic: Run instance in single network shared with others(deprecated). Now we have vpc for each customer. Classic link is for a link between ec2 classic adn vpc

- vpn cloudhub: provides secure conn between site if we have multiple vpn connections. Its via intenet that multiple vpn are connected though.

- transit gateway: Transitive peering between all our vpc and our corp network. Its a star topology and allows all diff vpc to interconnect. Regional serivce but can work cross regional.

- use private ip and same az for better savings of network cost.

## Disaster Recovery

- Recovery Point Objective (RPO): When in time to restore to. So time of disaster and the RPO determines how much data we have lost.  Recovery Time Objectiv (RTO): time to restore. This time decides how fast we can get back up.

- 4 types of disaster recovery: Backup and Restore, pilot light, warm standby and hot site/multi site.

- Backup and Restore: High RPO, as we take backup periodically, high RTO as well. Low cost. 

- Pilot light: A small version of app is already running in cloud. So core part is already up. Faster than above one cause, we have critical systems up and running already. We just build on top of that. Mostly used for critical core systems

- warm standby: Full system running, at minimu size. We can scale it to production load when required.active-assive

- Multi site/hot site: Full site already running in backup. active-active failover.

- Database Migration Service(DMS): migrate db from on premise to aws.its quick, secure and resiliant service. On prem db available during the process. It suppors migration frmo same type of db to same type, or one type to another. Continous replication using CDC. This works by using an ec2 running dms, that gets data and inserts to target db. We need to set up replication at ec2. 

- AWS Schema Conversion Tool(SCT): To change scheme when db are different during migration.

- we can download amazon linux 2 ami as iso and run it on premise. Vm import/export allows us to export our runnning on prem vm to aws, and so create a DR system from that. AWS application discovery service gathers info about on prem servers for migration and track using aws migration hub. AWS server migration service to do incremental back up of on prem servers to aws.

- aws datasync: to move large data from on prem to aws. sync to s3, efs or fsx for windows.move frm NAS of file system using NFS or SMB. We can set up replication as well here. Using this we can also do efs to efs sync as well.


## Misc

- NLB has no security grp. All traffic goes to underlying instances here. If we use CloudFront , we need to use WAF to restrict ips or client. Also can use geo filtering here as well.

- High performance computing (HPC): We can use snowball, direct connect or data sync to move large amount of data.

- EC2 enhanced networking(SR-IOV): high bandwdith, high pps, low latency. We can use Elastic Network Adapter(ENA) or use intel 82599VF(legacy). Elastic Fabric Adapter, improved ENA works only for linux. Leverages message passing interface, bypasing linux os itself to provide great communication.

- aws batch to run batch jobs . aws parallel cluster is an open source cluster management to deploy hpc on aws.

- for bastion host we can only use NLB(layer 4). Also if we use NLB bastion host can reside in an private subnet


## Other AWS Services

- CICD: Codecommit for code repo in aws. Codebuild for jenkins in aws. For deploy its codebuild in aws. Beanstalk can deploy for us as well. And to control all this we can use aws codepipeline.

- CloudFormation: We cannot edit a CF template once uploaded. Upload a new version for this. Any template needs Resources as Mandatory part. Then it has parameters, mappins, outputs, conditionals and metadata. It also has references and functions. Stacksets allows to create/delete reources across multtiple accounts in multiple regions.

- ECS: Helps run docker containers in ec2 machines. it has ECS core: running docker in ec2, fargate: running docker in aws provisioned instances(Serverless) and eks: running docker in aws kubernetes(ec2); ECR: container registry hosted by aws

- iam roles and security are at ecs task level.

- for ecs : we fist define ecs cluster(set of ec2 instances), then ecs service:  application definition running on ecs cluster, then ecs task+definition: containers running to finish the tasks and IAM roles for each tasks.

- ALB is comonly used with ecs. It has port mapping, which is not in classic load balancer and  hence cannot be used.

- to set up ecs: in ec2 cluster install ecs agent and update config file in each instance. For if using ecs ready ami, update config file.config file at /etc/ecs/ecs.config

- in config file: ecs_cluster used to assign the instance to this cluster; ecs_engine_auth_data to pull images frmo private repo; ecs_available_logging_drivers for cloudwatch container logging; ecs_enable_task_iam_role for iam .

- ecr is integrated with iam

- for ecs and iam: First ec2 instance needs role to run ecs agent in it and access ecs service. Then each task inside it needs its own role to perform its task.

- eks supports worker nodes if we want to deploy to ec2 or fargate if we want serverless. So its simialr to ecs but with kubernetes . both finally do the same thing

- step functions:orchestrate lambda. All in json state machine definition.can use other services in addition to lambda. max execution time is 1 year. has clumsy human intervention step

- aws simple wrkflow service(swf): its not serverless. runs in ec2.co ordinate task among many applications.1 year max time. Has decision step and activity step. has built in human intervention step. This is legacy and above one is recommended. Use this if external signals intervene the process or child process needs to return data to parent.

- emr creates big data hadoop cliusters in aws. EMR has auto scaling and works with spot instances.

- aws glue: for etl.fully managed, serverless. uses Apache spark. can find out a schema from data set. automated code generation. sources Aurora, rds, redshift, s3. Sinks s3, redshift Glue data catalog is metadata of the schema

- opsworks: managed chef and puppet. use this for cross cloud usage.

- elastic transcoder: Convert video files. It has jobs, pipeline for jobs, templates for conversion and sns alerts.

- aws workspace: desktop in cloud

- appsync: sync data between mobille and webapps. uses graphql

## architecturing in aws

- 5 pillers: Operationl excellance, security ,reliability, performance efficieny, cost optimization

- aws well arch tool: Helps us set up reminders for arch review. Asks us questions to evaluate our setup and provide recommendations based on that.

- operational excellance: deliver great app, run and monitor app. Infra as code, annotate documentation, make small freq reversible changes, refine operational procedures with time. Anticipate failure.

- security: implement strong identity solutuin, least privilege.Traceable and security at every level.Automate security practises, protect data in transit and at rest. Provide minimal access to data for people. Run mock security events and be prepared.

- reliability: ability of a system to recover from issues. Test recover procedures, automate receovery setup. Scale horizontally only .stop guessing capacity. use auto scaling. 

- performance efficieny: use resources efficieny. Use advances tech its available. go global in mins with cloud, use serveless. experiment more often using cloud. Be aware of all aws offerings

- cost optimization: delivers services at lowest cost. adopt pay for what you use model (like lambda). measure efficeincy using CW, stop spending money on infra as aws does it for us, analyze expenditure, use managed services to reduce cost.

- aws trusted advisor: high level aws account assessment. It recommends in cost optimization, performance, security, fault tolerance, service limits.Core check free for all customers, can get weekly alert mails. Full trusted advisor available for business and enterprise








