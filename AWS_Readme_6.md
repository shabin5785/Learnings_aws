- EBS backed volumes are persistant while instance store is not persistant. EBS can be detached and then attached to other instances, though not shared at same time. For this we use EFS

- INstance stored instances cannot be stored as data is not persistant. So they are also known as ephemeral, which means lasts for a short amount of time.

- OpsWorks is an orchestration framework that uses Chef to work 

- AWS organizataion helps us to consolidate multipe AWS accouts to one organization and manage them
-  It has an ldap styled structure with each node may or may not having the AWS account.
- Policies can be applied at root level or at nodes. 

- For billing we have a paying account and then different accounts linked to it. We get only one bill split by diferent accounts. 

- Consolidated billing(above one) wil save as cost as we bill together. ITs cheaper as we club storage and instances together.

- we can only have 20 link accounts by deafult

- Cross account access allows us to work with different accounts in aws console. Helps us work productlively in multi account environment. We can sign in with one username and swtich after siging in

- using tags we can create resouce groups, by combinig all with same tags to one group, across all regions. it includes ec2, cache, ebs  etc etc. SO we can monitor similar instances together and at one place.


- vpc peer allows communication between two vpc. It works only within a single region. Aws uses existing vpc resources for communication and does not use vpn or private connection for vpc peer. Hence there is no single point of failure or bottleneck

- vpc peer does not support transitive peer. Also participating vpc shouldnot have overlapping CIDR 

- AWS direct connet does not include internet. ITs a private connection to aws. So its slower to provision compared to a vpn

- STS : security token service grants users limited and temporary access. Users can be from Federal Access (like LDAP using SAML), Federation with mobile access(OPen id like Google, fb),cross acount access. THe users wont need an IAM id.

- How ldap login with aws works . User logs to Active Directory server (using ldap). On success, adfs returns a SAML assertion auth token. This is send to AWS IAM to sign in to aws, and if success redireccted to aws console. All this happens behind the screeen.

- AWS workspace is a relacement of a traditional desktop. They persistant and data is backed up( D drive) .So its VDI in cloud.Dont need aws account to use this.
