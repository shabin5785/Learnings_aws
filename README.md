# AWS_Learnings
Notes from AWS Course

**Compute Services**

- - Edge location are AWS endpoints used for caching data, typically this is cloudfront , amazons CDN. MOre edge locatins than regions.

- Each region has 2 or more avaiablity zones as back ups.

- EC2 : elastic compute cloud. These are virtual machines. Can have physical machines as well
- EC2 container service: where we run the containerized applications
- Elastic Bean stalk: This will auto provision our app, like load balance, bring up instances etc. Easy way to deploy and manage our app. AWS handles and provisions our app here

-lambda : It is jsut the code that runs. No deployment. Like a functin in cloud thats executed. Like a code to print text on pics, we can invoke lamba code wen someone upoads the image

- Lightsail : Amazons virtual private service. (VPC). 

- Batch : for batch computations

**Storage services**

-S3 : Simple Storage Service: For storage. object based storage upload into buckets.
 - EFS: elastic file system :network attacehd storage 
 - Glacier: Data archival
 - Storage gateway  :what we install in our site to replicate data to aws
 
 **Databases**
 - Elastic cache: Caching
 - RDS: relational database service (mysql,mssql, postgresql, oracle)
 - Dynamo: no relational
 - Red Shit : For data warehousing

**IAM**

- IAM: Policies are like permissions that can be applied to Users, Roles or Groups. Polices sits on top of these and governs what permissions each of them has

- IAM is global and not region specific. Once created its available throughout all regions

- Default linnk for IAM has the link with our user account number. WE can customize it to have a more user friendly name.

- Root account: The login with the email used for creating AWS account. So better safe with it. AWS by deault deletes the root access keys . We should for safety set up the multi factor authentication to the root account.

- User access type is of two kinds: Programatic where user enters an id and key and access through console. Second is via aws console .Users can have both access

- Once we create a user, we need to add them to a group and then add policies to teh groups or users, This is best practise of managing users. We can also attach individual polices to a specifc user as well.

- Access key and secret access key are shown only once, when we create the user. Once we confirm the createion, they cannot be retireved. So save it or email it to user. We can disable the access if we wish. then we can regenerate the access, which wwill create new access id and secret key (secret key is shown only once)

- Permission can either be directy applied (specific to user) or via group.

- We cannot assign role to an user. Role is a set or predefined policies appied to aws services. So if a EC2 instance has a role with permission of admininstrator, then via that EC2 instance we can controll the entire aws .

-An IAM role is very similar to a user, in that it is an identity with permission policies that determine what the identity can and cannot do in AWS. However, a role does not have any credentials (password or access keys) associated with it. Instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it. An IAM user can assume a role to temporarily take on different permissions for a specific task. A role can be assigned to a federated user who signs in by using an external identity provider instead of IAM. AWS uses details passed by the identity provider to determine which role is mapped to the federated user.

- I create a role and then can assign it to another aws services or users from other sign ins etc. Its  like a security access badge we pass to other people whom we wish

-Roles allow you to delegate access with defined permissions to trusted entities (like EC3) without having to share long-term access keys

- We can set billing alerts in aws. Cloudwatch is the aws service that can monitor our servies and alert us on billing


**S3**
-Simple Storage Service. Data can be retrived using a service from anywhere in the world.it provides a simle durable and and highly sclable object storage mechanishm

- its object storage. we cannot install apps or run os in s3

- files are stored in buckets. BUckts are like folders. 

- S3 has universal namespace, so unique names globally

- We get a url for our S3 bucket, which we can use to access it

- Read after write COnsistency for puts of new objects. Eventual consitency for overwrite Puts and Delete
So first case of new object put is immediate consistency. Next read after write will be the new object
Bt overwrite and delete might be inconsistent till changes are propoagated

BUt second one is always atomic. We either get new or old one. Never a corrupted or partial one

- S3 is object based, having a key and value (Data). iT has a version id for objects and a metadata for objects
(like date of upload, last updated by). It has subresources that exists under objects, like access control lists, torrent etc. 

-S3 is lexogaphhical. it orders objects by name in buckets. So if we have many objcets in our app, like a log file, that always begins with same name, but different endings(like timestamp), all will be put to same bucket leading to performacne issue. So to avoid this we can add a salt or random string to start of file name so that files go to different buckets

-S3 has 99.99 avaliablity

- S3 has tiered storage, like 30 days at one locaton, move to archive after taht etc
- it has encryption ,versioning and life cycel management. 
- we alos have access control lists and bucket control policies

- S3 - IA(infrequent access): less freq accessed, , lower fee than s3 but costs to retrive. But data is availale much much faster

- Glacier  : very cheap, for achival, takes 3-5 hrs for restore

- Reduced redundancy storage - less availablity for common and less critical data

- amazon transfer accelaration : fast storage to s3 from end user. Data is routed via edge router to s3 using optimum network path. So data first goes to edge locations from end user and from there to s3, not directly to s3 from end users. WE need to turn this on to be availale to use

-S3 is globally managed and not region specific

- So bucket name has to be globally unique. again like IAM s3 has a url where we can access it .So it has to be unnique.

- by deault all s3 buckets are private

- s3 encyption means aws stores data as encrypted data. There is also client side encyrption as well

- we can add tags to s3 objects or buckets. BUt tags are not inheretied from buckets downwards. 

- we can set policy on a bucket to contrl what people can do in a bucket

- Once we enable versionining in S3, we cannot turn it off. SO think and turn it on. Like big files of multiple versions can lead to too much space usage.

- we can have mfa for versioning so that we can control adding new objects or updations

**S3 Cross region replication**

- For cross region replications, s3 buckets should have versioning turned on
-- we can change the storage of the replicated buckets ,Like make the replication s3-IA or redunant type

-Now we need to create a role for the replication,as buckets need permission. So we can auto create a cross region replication permitted role. Main bucket will ahve this role. THis role properties allows replication from one bucket to another. (bucket names will be in role description).

- After replcaition turned on, new objects are replicated. 	Old ones are not by default replicated .We can do this from aws commmand line.

- Permissions are not replicated across s3. This is true for old data and new changes..

- with versioning , delete puts a delete marker on file. WE might have to specifically delete the versions as well.( delete is like a new version, but hidden from then on)

- above is true for replcation as well. delete is replicated and a marker is put on the file

- But if we delete the delete marker version, its nor repicated acroos. Replcation still has the delete marker version

- Version controls is not readliy replicated . We might have to delete teh versions.

- Life cylce management in S3 we can set rules for auto archival for files. like move to infrequent storage and then to delete. Same can be done for versions, like what to do with old versions, when new versions are uploaded. Also set up rules like delete incomplete file upload after certain days after a wait period

- we can tag s3 buckts, which is useful like tagged questiosn. to apply rules or search..

- glacier is used to archive old data stored

- Cloud front is the amazon CDN. Distribution is a collection of edge locations for content delivery.it can be web distribution for websites or rtmb distribution for video files

- we can put files to cache. It will update teh origin of the cache , a s3 or ec2 like that

- to delete from cache, first we need to disabel the object and then delete.

- S3 encryption :
	-In Transit (From pc to s3, using SSL/TLS)
    - At Rest : Server side encryption : S3 Managed keys (Keys are also encrypted by AWS):Known as SSE-S3
    - SSE-KMS (SSE with key management service)
    - SSE-C (sse with customer provided key)
    
   - Client side encryption; encrypt at client and then upload to 
   
- Storage gateway is like a Virtual Machine that acts as a layer between our data centre and s3. its a secure and fast way of sending and retriving data

- gateway can be File gateway(NFS), volume gateway(iSCI) which can be Stored volumes and Cached volumes, Tape gateway (VTL)

- Each of the three above is servicing the different types of storage that we have already present in our system. Like backup tapes is for VTL,flat files for FIle gateway etc

- Storage gateway can be connected to s3 using internet, direct connect or amazon VPC

 **Snowball**
 
 - a physical device from amazon that can be used to transfer large amount of data to AWS. Amazon gives an applicane where we can load our data, send to amazon and is loaded to aws by amazon. Its less costly than using an internet to load data to aws. Snowball uses encryption and tamper resistant enclosure for safety
 
 - Snowball edge is like snowball but has compute capabilites. So we can set it as a temporary storage set or temporary execution layer to support work at remote lcoatins. It can run lambda fuctions.
 
 - Snowmoile : Its a massive container on a truck that can store huge huge volumes of data
 
 - To transer data to snowball device, we need a credntial that we get from snowball page. Then we have a snowball client that we can use to interact with the device, once we connect it to network and power it on. It comens with a kindle for interaction
 
 - As S3 locatins are few and far away, we can use S3 transfer accelaration. We can upoad to an edge lcoation and then it will send it to our s3, thorugh amazon network, this is much faster. We get a url to an edge location, whcih can be used to upload. The edge cachce is choosen based on our lcoation. We can also compare direct upload and edge cache upload on speed and choose one.
 
 - On running a static site from s3, the bucket name needs to be same as the domain name if we are using amazon route53 with this. Its just a static website, no dynamic content.
