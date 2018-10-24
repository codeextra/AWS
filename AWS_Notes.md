EC2

Security Group
- Security Groups are stateful (means if inbound rule exists and no outbound rule, still response will be sent out. 
- All inbound traffic is blocked by default and we can't custom deny (only Allow)
- All outbound traffic is allowed by default 
- Can't block specific IP address traffic with Security Group whereas NACL can do that
- NACL's are stateless (there should be a corresponding o/b rule for each i/b rule)
- Default monitoring is 5 mins interval and can be increased upto 1 min

EBS
- 3 Types (GP2, IOPS, Magnetic Tape)
- Root volumes are attached to EC2 by default and can ONLY be encrypted thru copy of AMI
- To move EC2 volumes to a diff AZ/region take a snapshot/image of it and copy.
- EBS volumes won't be deleted when EC2 is terminated, only root is deleted.
- EBS volumes MUST be in the same AZ as EC2
- EBS volume size can be changed on the fly (recommended to stop and change)
- EBS and its snapshots are encrypted by default. Unencrypted snapshots can be shared or made public(can even sell in marketplace)
- Instance store volumes are sometimes called Ephemeral Storage (Temporary storage)
- By default both root volumes (EBS / IS) will be deleted when instance is terminated but with EBS we can choose to keep it

ELB
- Application LB, Network LB, Classic LB
- ALB operates at Layer 7 to load balance traffic across web servers
- NLB operates at Layer 4 to load balance TCP traffic (used for extreme performance)
- 504 error indicates load balancing error
- X-Forwarded-For header will indicate the end user source IPV4 along with the load balancer

ECS
 - Amazon's managed EC2 container service
 - Containers are created from a read only template called Image
 - Images are stored in ECR 
 - Task Definition (JSON) 
 
Volumes
 - EBS can be detached and reattached to instances whereas Instance Store can't be detached
 - EBS is persistent and instance store backed volume is not (ephemeral)
 - EBS volumes can be stopped ,data will still persist whereas instance store is not
 - EBS is long term storage and instance storage is short term

Resouce Grouping and Tags
- Tags can be inherited
- RG is grouping based on Tags
- Two types of RG : AWS Systems Manager group (per region and enables automation) and classic rg (across world)

VPC Peering
- Can peer across regions now
- Transitive VPC peering not allowed
- Cannot create VPC peering if CIDR ranges overlap

Opsworks
- Orchestration service using Chef /cookbooks/recipes
- Elastic transcoder is a media transcoder and does automatic to laptop/phone

AWS Organizations (Think of Sandbox)
- Manage multiple accounts at once
- Centrally manage policies across multiple AWS accounts
- Control access to AWS services (Service Control Policies(SCP) like deny specific service in an Account )
- Automate AWS account creation and management
- Consolidate billing across multiple AWS accounts

Cross Account Access
- Can access cross accounts by creating. Create "read-write-app-bucket" policy (prod), create "UpdateApp" cross account role(prod) and create new inline policy (Dev)

Security Token Service(STS)
- Federation - combining a list of users in one domain (IAM) with another (AD/FB)
- Identity Broker - service that allows u to take an identify from point A and join it (federate) to point B. IB always authenticates with LDAP first ,then with AWS STS and then provides temporary access to AWS resources
- Can u authenticate with AD ? yes using SAML
- Order of authentication - AD first and then temporary code

Workspaces
- Basically a VDI
- Windows 10 and can be personalized like wallpaper
- Use it like laptop (install applications, Data in D Drive is backed up every 12 hrs)

VPC
- VPC peering does not support edge to edge routing
- When u create a custom VPC these are created by default - SG, ACL and Route Table
- ONLY ONE Internet Gateway(IG) per VPC
- Security groups act like a firewall at the instance level, whereas NACL's are an additional layer of security that act at the subnet level
- /16 offers largest range of internal IP addresses
- Upto 5 VPC's allowed in each AWS region
- In Amazon VPC, an instance does not retain its private IP.
- Security Groups operate at the instance level NOT instance level
ACL's
- VPC has a default NACL with allows all inbound and outbound traffic
- Custom NACL's by default denies all inbound and outbound traffic
- Each subnet is associated with ONLY ONE NACL but a NACL can associate with multi subnets
- NACL's are stateless (SG's are stateful) and so u must always create inbound and outbound rules
- Rule numbers are always in increments of 100 (100 - HTTP, 200 - HTTPS, 300 - TCP),  lowest number rule takes precedence
- Block IP addresses using NACL not SG's

VPC Flow Logs
- Capture all of n/w traffic (IP traffic) and stored under cloudwatch Logs
- Flow logs can be created at 3 levels - VPC, Subnet, N/W Interface level
- Cannot enable flow logs for VPC's that are peered with ur VPC unless the peer VPC is in ur account
- Flow logs can't be tagged
- After a flow log is created the configuration cannot be changed e.g. u can't associate a different IAM role with the flow log
- Not all IP traffic is monitored, there are exceptions

NAT vs Bastion
 - NAT is used to provide internet traffic to EC2 instances in private subnet
 - Bastion is used to securely administer EC2 instances (SSH/RDP) in private subnets.


SQS
- Pull based whereas SNS is push based
- default message size is 256kbps
- retention period of 14 days
- visibility timeout is b/w 30s - 12 hrs )

SWF
- Used by Amazon.com to fulfill orders
- Actors : Starters,Workers/Actors and Decider. SWF brokers the interactions b/w Workers and Decider
- Task is assigned ONLY once and never duplicated , SQS allows duplicates
- SWF domains are registered in JSON format
- SWF is task oriented API whereas SQS is message oriented API
- SWF keeps track of all tasks and events in an application , SQS we need to implement
- In SWF,  "domain" refer to a collection of related workflows

SNS
- Subscribers - HTTP, HTTPS, Email, EMail-JSON, SQS, Application,Lambda
API Gateway
- It provides consistent RESTful application programming interfaces (APIs) for mobile and web applications to access AWS services
- Caching capabilities to increase performance
- Single Origin vs CORS (Cross Origin)

Kinesis
- Streaming data that is continuously generated like maps, fb, newsfeed
- Kinesis- Consume Big Data, Redshift - BI for big data, EMR - Processing of big data
- Kinesis Streams : Producers - Shards (retained for 24 hrs and can be increased upto 7 days) - Consumers
- Kinesis Firehose : Producers - Firehose (Analytics and takes care of consumers) - S3 - Redshift/ ELK
- Kinesis Analytics : Run SQL queries against streams/firehose
