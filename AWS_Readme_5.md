- AWS is responsible for securing underlying infra of the cloud, while we are responsible for the resources that we use in aws

- Amazon has procedure to decommision storage devices at the end of their life.

- aws protects against ipspoofing, sniffing, port scans, ddos, man in middle etc

- aws has a trusted advisor to inspect our infra and resources for vulnerabilities

- aws ensures data in same disk shared between differet instances are not sahred or accesible.

- aws has almost zero upfront cost, just in time infra, more resource utilization, usage based costing, reduced time to market. It allows automation of intrastructure, auto scalnig, pro active scaling, continous integraion, more efficient development cycle. Disaster recovery

- in cloud, assume that things will fail and always desing for disasters, be a pessimist while designing. Recovery , cut over, failures etc. Netflix has tons of softwares for this..  make things go wrong .. 

- make components decoupled as possible..so less dependant on each other..

- SCaling can be done as: Proactive Cyclic scaling, Proactive event based scaling and auto scaling.


**Framework**

- design based on security, performance efficiency, cost optmiziation, reliability and operational excellance

- stop guessing the capacity needs
- always test at production scale 
- automate to make it easier to test and experiment
- design to allow for evolutionary architecture
- test through game days, like test based on certain days load like black friday or end of year day etc

**Security**
- trace all events 
- automate response to all security events like hacks or breaches
- automate security best practices, harden our os, and use it for prod
- aws has shared responsibiity to security. AWS owns security for part and customer for the other

- Security consists of : Data protection,privilege management,infrastructure protection and detective controls
- Data : always encyypt and least privilege access as default
-aws allows key rotation, use it.
- detailed logging is available.. use it
- use versioning of data to prevent accidental overwrite.
- use ssl for transfer data

- privilege management: include access control lists (acl), role based control and password management( rotation included, strenght), key  management, access keys, MFA

- infra management: security groups, port management, routing, subnets , public access, network security, VPC, nat, antivirus

- detectivei controls : cloud watch, cloud trail 

**Reliability**

- Test recovery procedures, automatice recovery from failure and notifications
- scale horizontally to increase capacity as needed
- failure management
- always build a reliabel architecture before expanding it
- aws by default set limits on provisioning to stop over pricing and over use by customers. Can contact aws to increase these limit. 
- how do we back up data?
- recovery options? restore? no down time?
- how to react to changes in demand

**Performance Efficiency**
- how to use the computing resources efficiently to meet requirement and how to maintain them on face of changing demands
- use serverless architecture
- it consits of compute,storage, database and space-time trade off
- choose the right kind of server for our app, like based on our requirements, how much ram, speed etc. We can even choose withotu a server using lambda.
- choose storage based on use, acccess, capacity, privilege, patterns of access like random or frequent, 
- same for database,based on performance, throughut, requriments etc
-space time tradeoff: how to keep data close, what part of data to be kept close, cache which data etc.

**Cost Optimization**
- use serverless to reduce cost
- benefit from economies of scale
- match supply with demand

**Operational Excellance**
- includes operational best practices and procedures to  managage production systesm
- how we respond to changes.
- document adn test our procedures

