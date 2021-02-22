**Lambda**

- lambda encloses hardware, code, data centers , everything
-lambda we can upload code and get it work, lambda provisions and manages servers for our code. We dont have to worry abt os, scaling, patching etc

- it can be run in event driven mode, where lamdba runs in response to an event. Or runs as a comoute service, in response to service calls

- so to handle these we dont need server or ec2 or cloud of our own or provision them.

- lambda scales itself. we dont  have to maintain loadbalancers

- echo alexa is using lambda. voice is sent to a lambda function and it responds.

- lambda fns are independent 1 function  =1 event

- we can choose the language to write the lambda fn like node or go or python

- there is a definite syntax for lambda fn in each of this language. AWS has an ide (cloud 9) where we can write code.

- once we write the code in the lang choosen, we need to add a trigger ( s3, or api gateway etc)

- for example, we choose the trigger for lambda as api gateway. We then create an api gateway method (say GET) then link it with our lambda( can be of any lang). Now after deployment, on clicking or invoking the GET method of the api gateway, it will internally create a trigger and invoke the lambda. Lambda will execute its code and return the result to the api gateway method (GET here). we will see this result as result of our method invocation. 


- SNS (Simple notification service) can be used to pass around notifications

- we can create security policies ourselves or use the existing ones

- on design aws website using different services, we might have to enable CORS as one service url might be different from other. Like Api gateway accessing S3 , both of them have different url .so cors is needed

- we can link  multiple lambdas together. Like call a service and insert a value, the service invokes lambda which inserts to dynamo and posts to a quueu. Another lambda takes from queue, translates it and posts to another queue. Another lambda takes it from there and updates the dynamo with translation. All wihtout a service.

**Route 53**

- alias name vs cname: almost similar, but alias name can be resolved to individual aws resource. always prefer alias

- Different routing in Route 53 : Simple, weighted, latency, failover, geolocation.
- Simple: Make request, route53 forwards to our webserver
- Weighted: If we have multiple websites at different geos, we can design a weight ratio to forward the requests to different geos. like 20% to one  80% to another.
-Latency routing: Route based on the route or webserver with the lowest latency,
- Failover routing:  Active passive setup, like route to different if one website healthcheck fails
- Geolcoation routing: traffic based on geo location of the user.

**Database**

- Aurora is the aws relational database
- elastic cache is aws caching : it supports Redis and Memcached
- DynamoDB is the aws nosql database
- Redshit is aws data warehouse.. or analytics
-

- after creating db in aws, make sure to open the port to teh services within aws. Configure security group for this.
We can set the port to be open to any isntances within the our security group for servers.

- for rds there are two different type of backups: Automated or database snapshots. Automated backup is enabled by default, backup to s3, size of s3 based on our rds size

- snapshots are done manually. They are available even after we delete the db . But automated ones are deleted when we delete the DB, unless we take a final backup before deletion, which will be available after deletion.

- Restored rds will be a new instance with new endpoint and new dns name

- Aws supports encryption at rest for rds. Encrypting an existin db is currently not supported. We have to ecnyrpt an iinstance and migrate db to that

- multi availabilty zone, (AZ) allows to have an exact copy for our DB in diff geo. Aws hanldes the replication and sync between them. In case of a failure, aws will automatically switch to new region db

- Read replica is a readonly replica of our db that can be used to improve performance. THey are used for scaling and not for disaster recovery. Read replica needs automated backup turned on. we can create 5 replicas, and we can have read replica of read replicas.

- Raed replica cannot be multi AZ. They have diff dns endpoints. We can convert read replica to own db, but this breaks the replciation.

**DynamoDB**

- supports in place scaling without downtime, while rds does not
- it supports both document and key-value models. ITs fast consistent single milli second retrieval
- always on ssd, spread across three geos
- Eventually consistant reads. Also supports Strongly consistent reads.

- charged based on write capacity units. which is 1 write per second. SO 1 write capacity= 1 write per second. Similarly there is read capacity

- Write costs are much larger here. compared to cost per $ for write

**Redshit**

- aws data warehousing solution
- we can start with a single node(160gb), or multi node(leader node plus compute nodes). Upto 128 compute nodes, leader  node handles config

- it is a columnar storage like cassandra. It supports automatic distribution of data, compresses data for storage and performance.

- Data can be encyrpted at rest or in transit
- not availale for Multi AZ
- can take snapshots and restore it to a different Geo

**ElasticCache**
-it is an in memory cache that allows faster data retrieval 
- it supports memcached and redis. with redis we get multi AZ.

**Aurora**

- will only run in aws. cannot be run locally
- its mysql compatable.
- its multi AZ, 2 copies of data is in at least 3 availablity zones.So total 6 copies of data
- it transparently handles the loss of nodes. It self heals the storage,scans the disk for errors and fixes it.
