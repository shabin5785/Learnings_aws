**VPC**

- its a virtual network cloud, where we can control everything related to teh network. We can control everything like subnet, access, ip, nat etc

- we can also creata hardware vpc, connecting our hardware to our cloud vpc

- our vpc will be within a geo. Within vpc, we will have a router, and routing rules, that route to individual instances via network acl, security group etc.

- inside vpc we have multiple subnets. Network acl is  used to connect to one subnet from our router. Router is conneted to outside world.

- one subnet cannot be in multiple AZ.  but security group can span across multiple subnets

- CIDR.xyz

- one internet gateway allowed per vpc

- we get subnet network access control lists

- aws create a deafult vpc for us when we staart up. All subnet within vpc has by default access to internet. thats why we can access instances by default.
-Eacn ec2 has both private and public ip by deafult. but for custom vpc we have only private ip, if we want it to be private.

- we can connect peer to peer from one vpc to another, via privaet network . We can do that between vpc across multipe vpc accounts.
-Peering is star shape. One central and 4 nodes to that vpc.

- security group are stateful, so in and out are together. ACLS are stateless. so need separate in and out controls.

**Creating VPC**
-we create vpc, its like a private network. On creating it, a Route table is created for that, a Network ACL is created, a security group is created. No subnet or internet gateway is creatd.

- one vpc can have multiple route tables. Default route table created has no subnet information. We either have to associate subnet to this route table, or create new one and associate route table.

- We can have differetn route tables for different subnets.

- within vpc we create subnet. vpc can be like 10.0.0.0 to 255. Subnet is a sub of that like 10.0.1.0
- aws reservers first 4 and last one of the subnet ip address. So we have 251 out of 256 address.

- once we have vpc, we create all aws instances within that. As one subnet is one AZ, we require multiple subnets in multiple AZ to have the redudandancy and backup.  So we create multiple subnets within a vpc, and create instances within them

- Route table is created by default for a VPC. But deafutl created route tables for our vpc has connection to the Route table that aws provides default to our account ,and hence our vpc is exposed. So we should create a new route table for securty and better control

- be default auto assign public ip is off for subnets. We have to turn this on for our internet facing subnet to connect from outside.

- now we dont want ot connect all our subnet to internet gateway. So we configure our route table and connect only one subnet to the internet gateway. Other one is private, but subnets are peer connected.

- security group cannot span vpc. So the one create by default goes to defautl vpc, and when we create our own vpc, we cannot use the created one

- be careful with security grp for private vpc. make sure it has restricted access.

- to connect between instances in two subnets, we just need to open ports and assign the permissible ip to be the subnet range. So we can create a security grp and create rules and add this to instance in private subnet

**Access internet from private subnet**

- For this we create a NAT instance, having access to internet.(ec2 instance). then we route our private subnet to this nat, and via this we get internet. AWS has a deafult rule that trafic in an instance should origin and terminate at the instance. Neeed to turn this off for Nat as it bypasses the trafic.

- the nat instance is not a good way, as we are dependant on a single instance for our network activities. better its to use nat gateway. Nat gateway is the preffered way 

- nat instance must always be in public subnet

- aws manages the nat gateway, makes it cluser, secure and avalable
- nat gateway is not associated with security groups, or disable source destination check. No ssh acccess to gateway, secure efficient.

**NACL**
- by deafult all commns is disabled when we create NACL

- epiphermal ports

- nacl rules are evaluated in numerical increasing order

- nacl we can block specific ports and ips

- one subnet can be associated with one nacl. but one nacl can be assocaited with multiple subnets

- if we dont creat a nacl, a default is auto created with all allowed.

- nacl can span multi AZ

- For app loadbalancer, with vpc, it needs subnet to be in two availability zones. And both of them shld be public as well.

-VPC flow logs : log of iptraffic in vpc, stored and analyzed at cloudwacth . Can be creatd at vpc , subnet or network interface level.

- bastion instance is used to securely adminster the instance within vpc

- vpc endpoint allows to connect a vpc to other services, over aws and not through internet. in vpc endpoint, we choose we wan s3 gateway or similar services.

- security group is at instance level while nacl is at subnet level


**Services Available in AWS**

- SQS is a distributed queue system
-messages can be upto 256kb

- There are two queues : Standard and FIFO
- Standard: More than one copy of message may be delivered. Messages cant be guaranted to be delivered in order
-FIFO: maintain order of delivery , no duplicates. They have all capabilities of standard queue, limited to 300tps, and allow multiple ordered message queues within it
- Messages can be kept from 1 min to 14 days, with 4 days default

- visibility timeout: time when message is invisible in queue after being read by a process. If job is completed before visibilty time, message is deleted otherwise it becomes visible again and other process may read the message

- visibility timeout max is 12 hrs

- SQS guarantees that mesasge is delivered at least once

- read from queue can be done as short or long polling. Short polling returns immediately while long polling waits for a message or till long poll timesout


**SWF : Simple Work flow Server**
- SWF allows us to control and coordinate actions across a distributed application components.
- SWF has 1 year retention of tasks
- SWF is task oriented api, while SQS is message oriented
- SWF ensures that task is delivered only once
- SWF has startes( that start the task flow), Deciders (that decide the work flow), Activity WOrkers(Actors) (that carry out the task)

**SNS : Simple notification Service**
- send notifications from appllocations and deliver to subscribers
- it supports push notifications to mobile devices,baidu cloud
- it supports push notificaitions to email sms, lambdas, sqs, or http endpoints
- it supports topics, an access point for subscription. One topic supports delivery to multipe modes of endpoints. We publish to SNS and it converts message to format requried for different end points
- SNS suports mutlti AZ, for redudandancy 

- SNS is pushed, instantaneous, (no polling) compared to SQS

**Elastic Transcoder**
- Convert media from original source to other format.

**API gateway**

- an easy way to creaet api that can be used to access services within aws, It is like a single point of interface to connect to whihc in turn connects to other services behind it.

- we can enable api caching in api gateway to enable faster response

- its low cost and scales effectively

**Kinesis**
- to handle streaming data, analyze it
- has kinesis streams, firehose and analytics
- we send our streaming data to kinesis streams. default 24 hrs to max 7 hrs retention. Data stored in shards. We can then read the data from this and use it for our purpose. Capacity and performance is dependant on shards 

- firehouse: no retention, analyze or store data as soon as it comes in. like to an elastic search cluster
it has no shards. 

- analytics allows analysis of data using sql queries in streams or firehose

**Draw.io**

- s3 sync command can be used to sync contents between s3 and ec2 or any other instances. Copying will copy all files irrespective of it exists in destination. and will not delete if source is deleted. But sync does both of this.
dryrun option?

- load balancer is a good option to have static dns name. as instances behind it can change ip, but with a load balancer we get a staic dns name

**Cloud Formation**

- it allows us to creata a cloud based platform from a atempalte. we can use predefined tempplates or use existing one
