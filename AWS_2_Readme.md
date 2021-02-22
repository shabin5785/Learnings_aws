**EC2**

- Elastic compute cloud : its a virtual machine on cloud, provisioned on demand.

-EC2 can be availled on demand ( by hour or second), reserverd(say for 1 or 3 years, less cost), spot(where we bid for ec2 instances time slots), dedicated hosts(physical server dedicated to user, mainly use for licensing as licensed software needs to be deployed and used in separate server or by govt or security regulations preventing sharing server with others, like in on demand ec2)

-reserved is used, when we can predict the load and decide on our needs. Pay upfront and have predicate usage.We can schedule a reserve so that it starts when loads are high. (like high traffic dates)

- spot is used for apps with flexible start and end times. we bid for a spot and use the ec2 at that time. 
- if we terminate the instance within our spot, we pay for it. But if aws terminates it as the price for the spot is above the bid price, we get the usage till then free.

**Storage types**
-D2 : dense storage  (2 is the second generation), hadoop, warehouse etc
-R4 : Ram , memory optimized (for memory intensive activites, databases)
-M4 : General purpose, usually the main choice (application servers)
-C4 : Compute optimized (CPu intensive apps)
-G2 : Graphics (video intensive)
-I2 : High speed storage (nosql, data warehouse)
-F1 : Field programmable gate array (we can change the hardware to suit our application)
-T2 : Lowest cost, general purpose, (webserver and small database)
-P2 : Graphics general purpose GPU (Machine learing, bitcoin mining)
-X1 : Memory optimied, extreme compute (Spark, HANA etc)

DR MC GIFT PX

**EBS**
- Elastic Block Storage
- S3 is object based store. we cannot run applications or install database in that. 
- EBS allows us to create storage volumes and attach them to EC2 instances. Once attached, we can create a file system in them, run a database. Is a block based storage. 

- We cannot mount one EBS to multiple EC2. For this use EFS

- EBS is placed in availablity zones, where tehy can be automatically replciated to protect from failures

1. S3 is a storage facility accessible any where
2. EBS is a device you can mount onto EC2
3. EFS is a file system you can mount onto several EC2 instances at the same time


Types:
- General purpose SSD( GP2): balance of price and performance.
- Provisioend IOPS SSD (IO1): designed for i/o intensive tasks
- Throughput optimized HDD (ST1): big data, log processing, data warehousing.It cannot be a boot volume
- Cold HDD(SC1): Lowest cost for infrequently accessed. Cannot be a boot volume as well
- Magnetic Standard:Lowest cost of all thats bootable. For infrequent access and low cost

- EC2: first we need to select the based image of our instance(linux distros, amazon image or windows etc). THey are known as Amazon Machine Image

-Ec2 one subnet is for one availability zone

- by deault ebs attached to ec2 is deleted on ec2 instance termination

- EC2 on creation has a inbuilt storage , root. We can attach additioanl ebs storage to it
- We can put tags on EC2 instances. This helps us to identify and control instances

-EC2 security group is a virtual firewall. A security group is a set of firewall rules that control the traffic for your instance.  Here we can set up whether we want SSH, HTTP etc, and whihch ip address range for these to be allowed from, which port to be used etc.

- ec2-user is the default user to connect to an ec2 instance. we then need a private key to connect to that as well
use  puttygen to generate ppk fiel and use it in ssh. there is a root user in the instance, we can switch to root within the instance.

- we can have a private ip and address as well as a public ip and address for an instance. (address is like a url, whihc is dns resolved to the ip

- by deafualt we cannot encrupt root volume. to do that, we need to create instance, then make an amazon image of it, encrypt it and then use it as base for other instances.

**Security Groups**
- we can have many security groups between public and our ec2 instance

- Any change to security group will be aplied almost immedieatly

- security group has inboudn and outbound rules. Now SG rules are state bound. Anything inbound is allwoed outbound as well. We can remove http or everuthon from outboound. but as inbound http is allowed, and http is stateful outbound is allwoed by default

- everything is blocked in SG by default. ALl are disallwoed at start. We have to enable ones we want

- there is a default SG, which allows connections between instances under same SG.

- there will be no conflicts when we add multiple SG to an ec2 instance, as there can be no deny rules, so rules of all SG are added together to get the rules for the instance.

- EC2 instance and EBS volume has to be in hte same availability zone.

- To Move EB2 to new region, create snapshot of volume, create image from that, copy image to new region, then in new region we can use it for instance creation.

- we can create image from our runnign instance as well

- Attached ebs volumes wont be terminated by default, even when we termiante the ec2 instance.

- snapshots are incremental. So only the changes are put into the new snapshots

- AMI can be created from Volumes or Snapshots. Now if the snapshot is not of a volume with that allows image to be created, we wont be able to create AMI from snapshot

- We can change the size and type of EBS volumes (better change it when machine is not runnig)

- snapshots from encrypted volumes are encrypted themselves, volumes restored from encypted snapshots are also encrypted. We can share snapshots only if there are un encrypted

- FOr windows ec2 instance, admin pasword needs to be geneartd. This can be done using the keyfile from aws console

- we can encrypt a volume by taking a snapshot and then copy it to a different region(during copy we get encryption option). Now we create an AMI from that encrupted volume, The instance launched from this is encrypted , at root storage as well.

- From amazon market place ,we get buy pre configure AMI, free and paid. We can use to lauch our instances.

- to take snapshot of root device, we need to stop the instance first.
- we can share only unencrypted snapshots

**EC2 instances**

- we can choose ec2 based on region, availabity zones, os, architecture(32 or 64) , launch permissions or storage for root device(root volume device)
- last one can be instance store(ephermal storage) or ebs backed volume

- majority of AMI has root device type EBS backed.

- with instance store ec2, we cannot stop instance, while we can do that with ebs store. So if inssance store ec2 fails, we lose the data. Ebs backed can be stopped and not lose data. Reboot is possible for both and both will  not lose data. On termination of ebs backed, we can ask aws to keep our root volume, whihc is not possible in instance backed


**Elastic Load Balancing**

- Three types of load balancers : APplication (http, https), network(tcp) and classic(deprecated)

- Creating tags helps us track usage using resoirce groups. SO give meaningful names

- We dont get a public ip for load balancers. ONly get the DNS. Amazon manages the ip internally

- For app load balancer, we create a target group for the load balancer to connect to.

- We then register the instances we want to monitor to the target group

- be careful about load balacner settings liek threshold, when to take a device ofline, how many failures allowed etc

- cloudwatch allows us to monitor aws resources (ec2, load balancer etc) and create dashboards and alarms 
- also cloudwatch events allows us to respond to state changes and allows us to take action. Like invoke a lambda fucnction to be run when an ec2 isntance comes up , so that we can update our domain name with the new ip address

- we can add metrics to cloudwatch dashboards to monitor or text.

- we can also monitor logs, for this we need to isntall a agaent in our isntances to get logs to cloudwatch

- Cloudwatch is for app monitoring and cloudtrail is for auditing the aws instances
-

**CLI**

- accessing cli from ec2 can be done using secret keys or by roles. Using keys, we have to configure ec2 cli usign the keys, This causes the keys to be stored within ec2, so if someonce hacks to our ec2, they can get hold of the key. Much safer way is through roles.

- Should probably never configure cli from ec2 usign keys, if did that, better delete the users

- best way is to create a role that can access cli, and assign that role to teh ec2 instance that we need

- Roles are created globally

- we can attach or edit roles to a running instance

- with roles aws cli works without need for configuration. INstance on creation has the permission through the roles .
- we can change roles and reflect on ec2 at once, no need to store it locally. etc

- we can still configure an aws instance with role. We can leave the keys empty as that is taken from the role. And no data is saved to ec2 as well. 

- roles allows us to apply the permission to multipe ec2 instancs at once.

-its better to use the region flag in aws cli to work with s3 locations 

**Scripting**

- bash script is the set of commands to be run on ec2 when it starts up or boot

- we write the bash commands while creating the instance.

**Ec2 instance metadata**
- from aws cli in ec2, run below command to get metadat
 curl http://169.254.169.254/latest/meta-data/

- metadata is a set of values and variables for the insances,like public ip , id, etc
append the parameter to end of the above link

- we can put these metad data commands to a bash file and get all details when ec2 instances. and then upload it automatically etc.. then parse it.. use it.. all linked together.

**Autoscaling**

- to crete autoscaling group, first we need to create a lauch configuration. This is the template the auto scaling group will use to lauch new instances.

- in the launch config, we can set up the bashscript to be run on new instances spawn up. We can now make it same as the existing ones..

- after lauch config, we need to create auto scaling group and polices

- in auto scaling config, we need to link the scaler with our load balancer. Only then it can decide on when to auto sacle based on the data from load balancer.

- important to check the healthcheck time. give sufficent time for new instance to spawn up

- we can then decide when to start teh group, increase it or decrease it etc in the auto scale properties

- So link is a a launch config to start new instances, then a scaling config, the scaler is connected to load balancer, we then decide the config for when to scale up and down, how much etc.

**Placement Group**
- a logical grouping of instances in single availability zone. Done for low network latency, high network throughput. they cant span over multiple availablity zones

- name of placement group must be unique within our aws account

- only allows a specifc type of instances in placement groups

- Aws recommends homegenours (instances of same type) withi placement groups

- you cannot move placement groups, can create ami and move it

**EFS**

- efs is a file storage for ec2
- efs is elasic. growing and shrinking as needed
- efs can be mounted to multipe ec2 instances
- pay for the storage that we use. not like ebs where we need to decide before hand abt storage
- support nfs(network file storagE)

- after creating an efs, we can mount the same to an ec2, also mount same efs to multiple ec2 instances. so files in snigel location but served to different ec2 instances.
