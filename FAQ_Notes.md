### IAM
- Can manage federated users. Its free of charge and any aws user can use it. 
- By default, users have no ability to call service APIs on behalf of the account.
- IAM users can have any combination of credentials that AWS supports, such as an AWS access key, X.509 certificate, SSH key, password for web app logins, or an MFA device.
- You can enable and disable an IAM user's access keys via the IAM APIs, AWS CLI, or IAM console. If you disable the access keys, the user cannot programmatically access AWS services.
- You can organize users and groups under paths, similar to object paths in Amazon S3—for example /mycompany/division/project/joe.
- User names are just ASCII strings that are unique within a given AWS account. You can assign names using any naming convention you choose, including email addresses.
- All limits are on the AWS account as a whole. For example, if your AWS account has a limit of 20 Amazon EC2 instances, IAM users with EC2 permissions can start instances up to the limit. You cannot limit what an individual user can do.
- You assume an IAM role by calling the AWS Security Token Service (STS) AssumeRole APIs (in other words, AssumeRole, AssumeRoleWithWebIdentity, and AssumeRoleWithSAML). These APIs return a set of temporary security credentials that applications can then use to sign requests to AWS service APIs. There is no limit to the number of IAM roles you can assume, but you can only act as one IAM role when making requests to AWS services. An IAM role does not have any credentials and cannot make direct requests to AWS services. IAM roles are meant to be assumed by authorized entities
- Cannot add iam role to a group as of now

- You can add as many inline policies as you want to a user, role, or group, but the total aggregate policy size (the sum size of all inline policies) per entity cannot exceed the following limits:
User policy size cannot exceed 2,048 characters.
Role policy size cannot exceed 10,240 characters.
Group policy size cannot exceed 5,120 characters
For managed policies: You can add up to 10 managed policies to a user, role, or group. The size of each managed policy cannot exceed 6,144 characters.

- You are limited to 1,000 IAM roles under your AWS account (soft limit)

- IAM roles for EC2 instances provides the following features:
AWS temporary security credentials to use when making requests from running EC2 instances to AWS services.
Automatic rotation of the AWS temporary security credentials.
Granular AWS service permissions for applications running on EC2 instances.

- can change/delete role of a running machine.
-  You can add an IAM role as an additional parameter in an Auto Scaling launch configuration and create an Auto Scaling group with that launch configuration. All EC2 instances launched in an Auto Scaling group that is associated with an IAM role are launched with the role as an input parameter
-You can only associate one IAM role with an EC2 instance at this time. This limit of one role per instance cannot be increased.

- The AWS temporary security credentials associated with an IAM role are automatically rotated multiple times a day. New temporary security credentials are made available no later than five minutes before the existing temporary security credentials expire.

- A service-linked role is a type of role that links to an AWS service (also known as a linked service) such that only the linked service can assume the role.

- Use managed policies to share permissions across IAM users, groups, and roles. Like attach it to all of these.

- IAM policies are evaluated together with the service’s resource-based policies. When a policy of any type grants access (without explicitly denying it), the action is allowed.

- Managed policies can only be attached to IAM users, groups, or roles. You cannot use them as resource-based policies.

-  Using IAM roles, IAM users and federated users can access resources in another AWS account via the AWS Management Console, the AWS CLI, or the APIs

- The IAM policy simulator is a tool to help you understand, test, and validate the effects of your access control policies. The policy simulator is available to all AWS customers and is free.

- The account alias is a name you define to make it more convenient to identify your account. You can create an alias using the IAM APIs, AWS Command Line Tools, or the IAM console. You can have one alias per AWS account.

- Temporary security credentials consist of the AWS access key ID, secret access key, and security token. Temporary security credentials are valid for a specified duration and for a specific set of permissions. Temporary security credentials are sometimes simply referred to as tokens. Tokens can be requested for IAM users or for federated users you manage in your own corporate directory.

- IAM users can request temporary security credentials for their own use by calling the AWS STS GetSessionToken API. The default expiration for these temporary credentials is 12 hours; the minimum is 15 minutes, and the maximum is 36 hours.

- temporary security credential cannot be revoked prior to its expiration

- Federated users (external identities) are users you manage outside of AWS in your corporate directory, but to whom you grant access to your AWS account using temporary security credentials. They differ from IAM users, which are created and maintained in your AWS account.  You can programmatically request temporary security credentials for your federated users to provide them secure and direct access to AWS APIs and also access management console.

-  One way is by programmatically requesting temporary security credentials (such as GetFederationToken or AssumeRole) for your federated users and including those credentials as part of the sign-in request to the AWS Management Console. After you have authenticated a user and granted them temporary security credentials, you generate a sign-in token that is used by the AWS single sign-on (SSO) endpoint. Alternatively, you can post a SAML assertion directly to AWS sign-in ( https://signin.aws.amazon.com/saml). 

- When you request temporary security credentials for your federated user using an AssumeRole API, you can optionally include an access policy with the request. The federated user’s privileges are the intersection of permissions granted by the access policy passed with the request and the access policy attached to the IAM role that was assumed.

- Depending on the API used to create the temporary security credentials, you can specify a session limit between 15 minutes and 36 hours (for GetFederationToken and GetSessionToken) and between 15 minutes and 12 hours (for AssumeRole* APIs),

- Web identity federation allows you to create AWS-powered mobile apps that use public identity providers (such as Amazon Cognito, Login with Amazon, Facebook, Google, or any OpenID Connect-compatible provider) for authentication. With web identity federation, you have an easy way to integrate sign-in from public identity providers (IdPs) into your apps without having to write any server-side code and without distributing long-term AWS security credentials with the app

- You can enforce MFA authentication by adding MFA restrictions to your IAM policies. 

### EC2
- By using Amazon EBS, data on the root device will persist independently from the lifetime of the instance. This enables you to stop and restart the instance at a subsequent time, which is similar to shutting down your laptop and restarting it when you need it again. Alternatively, the local instance store only persists during the life of the instance. This is an inexpensive way to launch instances where data is not stored to the root device. 

- Amazon EC2 is transitioning On-Demand Instance limits from the current instance count-based limits to the new vCPU-based limits to simplify the limit management experience for AWS customers. Usage toward the vCPU-based limit is measured in terms of number of vCPUs (virtual central processing units) for the Amazon EC2 Instance Types to launch any combination of instance types that meet your application needs. Starting September 24, 2019, you can opt in to vCPU-based instance limits 

- EBS snapshots are only available through the Amazon EC2 APIs and not via s3 apis

- If you share a snapshot, you won’t be charged when other users make a copy of your snapshot. If you make a copy of another user’s shared volume, you will be charged normal EBS rates.

- Metrics are received and aggregated at 1 minute intervals in CW min level.
-You can retrieve metrics data for any Amazon EC2 instance up to 2 weeks from the time you started to monitor it. After 2 weeks, metrics data for an Amazon EC2 instance will not be available if monitoring was disabled for that Amazon EC2 instance. Amazon CloudWatch stores metrics for terminated Amazon EC2 instances or deleted Elastic Load Balancers for 2 weeks. Amazon CloudWatch monitoring charge does not vary by Amazon EC2 instance type.

### ELB
-  WebSockets and Secure WebSockets support is available natively and ready for use on an Application Load Balancer.Request tracing is enabled by default on your Application Load Balancer.
- Can use a single Application Load Balancer for handling HTTP and HTTPS requests
- you can terminate HTTPS connection on the Application Load Balancer. You must install an SSL certificate on your load balancer.

- SNI is automatically enabled when you associate more than one TLS certificate with the same secure listener on a load balancer. Similarly, SNI mode for a secure listener is automatically disabled when you have only one certificate associated to a secure listener.
- IPv6 supported with an Application Load Balancer
-  You can integrate your Application Load Balancer with AWS WAF, a web application firewall that helps protect web applications from attacks by allowing you to configure rules based on IP addresses, HTTP headers, and custom URI strings. 
-  Cross-zone load balancing is already enabled by default in Application Load Balancer.
- You are charged for each hour or partial hour that an Application Load Balancer is running and the number of Load Balancer Capacity Units (LCU) used per hour.

- Network Load Balancer process both TCP and UDP protocol traffic on the same port
- you can create your Network Load Balancer in a single availability zone by providing a single subnet when you create the load balancer.
- Network Load Balancer support DNS regional and zonal fail-over
- For each associated subnet that a Network Load Balancer is in, the Network Load Balancer can only support a single public/internet facing IP address.
- You can enable cross-zone loading balancing only after creating your Network Load Balancer. You achieve this by editing the load balancing attributes section and then by selecting the cross-zone load balancing support checkbox

### ASG
- You should use EC2 Auto Scaling if you only need to scale Amazon EC2 Auto Scaling groups. Other wise use AWS ASG
- Predictive Scaling is a feature of AWS Auto Scaling that looks at historic traffic patterns and forecasts them into the future to schedule changes in the number of EC2 instances at the appropriate times going forward. Currently only for ec2. Its free to use and can work with even only 1 day data to two weeks data or more.
- we can scale ec2, ecs, ec2 spot feelts, dynamodb, aurora replicas

### EFS
- Amazon EFS offers a Standard and an Infrequent Access storage class. The Standard storage class is designed for active file system workloads and you pay only for the file system storage you use per month. EFS Infrequent Access (EFS IA) is a lower cost storage class that’s cost-optimized for files not accessed every day. Data stored on the EFS IA storage class costs 85% less than Standard and you pay a fee each time you read from or write to a file.
- Enable Lifecycle Management when your file system contains files that are not accessed every day to reduce your storage costs
