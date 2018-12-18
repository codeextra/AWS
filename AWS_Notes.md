Edge Locations
- Endpoints for AWS which are used for caching content. Typically this consists of CloudFront, Amazon's CDN (content delivery network)

IAM
- IAM is global , users /roles/groups/policies included
- username/pwd for console access and access id/security access key for CLI access (generated and emailed)
- Root Account has admin access
- ONLY one IAM role per EC2 instance

S3
- S3 bucket names are global
- By default, customers can provision up to 100 buckets per AWS account. However, you can increase your Amazon S3 bucket limit by visiting AWS Service Limits.
- Files are sized b/w 0 - 5 TB (only files not OS driven blocks like DB)
- Read for new files is instant whereas overwrite/update/delete is eventual consistent
- Security - Bucket Policy at bucket level, ACL's at file level
- S3 Standard (durable, multi-AZ, NO retrieval fee)
- S3-IA (Infrequently accessed data , but rapid access. Its cheaper than S3 but has a retrieval fee)
- S3 One Zone -IA (same as IA , even cheaper but don't require multi AZ)
- S3 Reduced Redundancy (Store non critical data)
- Glacier (Archival) - Expedited ,Standard or Bulk. Typical retrieval 3-5 hrs. NO SLA
- S3 Object has Key, Value, Version ID, Metadata, Sub-resources (ACL, Torrent)
- S3 has client and server side encryption( SSE S3, SSE-KMS, SSE-C)
- Transfer Acceleration by moving files to edge Locations
- By default buckets and the objects stored are PRIVATE
- MFA Delete to avoid accidental deletes
- Once enabled versioning can't be disabled, only suspended
- Cross region replication
- LifeCycle rules - Transition to S3-IA in 30 days and to Glacier in 60 days. Can permanently delete too
- S3 transfer acceleration uses CloudFront/edge location to fast transfer files to S3
- Can host a static website with format http://bucketname.s3-website-region.amazonaws.com

RDS
- Multi-AZ can only be used for failover and not for scaling. Read replica is the solution for scaling
- Read Replica DB's are asynchronous (only thru snapshot) whereas multi-az is synchronous
- DB Snapshots or automated backups CANNOT be taken of Read Replicas
- Dynamo DB is region specific (no need of Multi-AZ) and suitable for stateless web apps

EC2
- OnDemand / Spot/ Reserved / Dedicated
- FIGHTDRMCPX (pnemonic for instance types)
- Default metrics cover CPU, Disk, Network
- Termination protection is turned OFF by default
- Root EBS volumes cannot be encrypted by default , only copies of AMI can be encrypted
-

CloudFront
- Origin for cloudfront can be S3,EC2, ELB, Route53 and are cached in edge Locations
- 2 types of distributions (collection of edge locations) - Web distribution and RTMP (media streaming)
- Edge locations are not read only (PUT allowed)
- Objects are cached for the life of TTL
- Cached objects can be cleared too, but will be charged
- To restrict users from accessing S3 or a url use cloud front pre-signed url's or cookies
- Geo-restrict files by blacklisting/whitelisting in cloudfront
- Invalidation - If u pushed a file to edge location it lives for 24 hours (TTL), but if u want to get rid of it invalidation is the option

Storage Gateway
-  AWS Storage Gateway is a hybrid storage service that enables your on-premises applications to seamlessly use S3 storage in the AWS Cloud.
- 4 types - File Gateway (NFS), Volume Gateway (iSCSI)- Stored and Cached, Tape Gateway (VTL)
- iSCSI is a block based protocol.
- File Gateway - For flat files, stored in S3
- Volume Gateway :
  Stored - Entire dataset is stored on-site and is asynchronously backed up to S3 (EBS Snapshots)
  Cached - Entire dataset is stored on S3 and most frequently accessed is cached on-site
- Virtual Tape Library(VTL) - Used for backup and uses popular backup apps like NetBackup,Vaeem
- Redshift : best suited for data warehousing

Security
- Cognito : Mobile App users temporary access to AWS
- GuardDuty : threat detection service that continuously monitors for malicious or unauthorized behavior to help you protect your AWS accounts
- Inspector : Amazon Inspector enables you to analyze the behavior of your AWS resources and helps you identify potential security issues.
- Macie : Data visibility security service that helps classify and protect your sensitive and business-critical content.
- AWS Organizations: you can create Service Control Policies (SCPs) that centrally control AWS service use across multiple AWS accounts.
- AWS WAF and AWS Shield help protect your AWS resources from web exploits and DDoS attacks

Security Group
- Security Groups are stateful (means if inbound rule exists and no outbound rule, still response will be sent out.
- All inbound traffic is blocked by default and we can't custom deny (only Allow)
- All outbound traffic is allowed by default
- Can't block specific IP address traffic with Security Group whereas NACL can do that
- NACL's are stateless (there should be a corresponding o/b rule for each i/b rule)
- Default monitoring is 5 mins interval and frequency can be increased up to 1 min

EBS
- 3 Types (GP2, IOPS, Magnetic Tape)0
- GP2 < 10000 IOPS, IOPS > 10000 IOPS
- Root volumes are attached to EC2 by default and can ONLY be encrypted thru copy of AMI
- To move EC2 volumes to a diff AZ/region take a snapshot/image of it and copy.
- EBS volumes won't be deleted when EC2 is terminated, only root is deleted.
- EBS volumes MUST be in the same AZ as EC2
- EBS volume size can be changed on the fly (recommended to stop and change)
- EBS and its snapshots are encrypted by default. Unencrypted snapshots can be shared or made public(can even sell in marketplace)
- Instance store volumes are sometimes called Ephemeral Storage (Temporary storage)
- By default both root volumes (EBS / IS) will be deleted when instance is terminated but with EBS we can choose to keep it
- EBS snapshots can only be shared if Unencrypted
- EBS can be detached and reattached to other EC2 instances
- Instance Store / Epheremeral are non-persistent meaning they will be lost when EC2 is terminated
- EBS (D drive) and Instance Store (C drive)
- EBS Backed = long term Storage
- Instance Store = stort term storage

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

Resource Grouping and Tags
- Tags can be inherited
- RG is grouping based on Tags
- Two types of RG : AWS Systems Manager group (per region and enables automation) and classic rg (across world)

VPC Peering
- You must have a VPC with Hardware VPN Access, an on-premise Customer Gateway, and a Virtual Private Gateway to make the VPN connection work.
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
- Identity Broker is built by customer (not AWS out of box)
- Identity Broker - service that allows u to take an identify from point A and join it (federate) to point B. IB always authenticates with LDAP first ,then with AWS STS and then provides temporary access to AWS resources
- Can u authenticate with AD ? yes using SAML
- Order of authentication - AD first and then temporary code

Workspaces
- Basically a VDI
- Windows 10 and can be personalized like wallpaper
- Use it like laptop (install applications, Data in D Drive is backed up every 12 hrs)

VPC
- You can have:
5 Amazon VPCs per AWS account per region
200  subnets per Amazon VPC
5 Amazon VPC Elastic IP addresses per AWS account per region
1 Internet gateway per VPC
5 virtual private gateways per AWS account per region
50 customer gateways per AWS account per region
10 IPsec VPN Connections per virtual private gateway
- 3 Diff IP ranges - 10/8, 172.16/12, 192.168/16
- VPC creation by default creates NACL, SG and Route Table. NO SUBNET is created default
- First 4 and last one IP address are reserved and cannot be used. So always 5 addresses are lost
- Only ONE IGW can be attached per VPC
- VPC Peering is thru a central hub and NO Transitive peering
- VPC peering does not support edge to edge routing
- When u create a custom VPC these are created by default - SG, ACL and Route Table
- ONLY ONE Internet Gateway(IG) per VPC
- Security groups act like a firewall at the instance level, whereas NACL's are an additional layer of security that act at the subnet level
- /16 offers largest range of internal IP addresses
- Upto 5 VPC's allowed in each AWS region
- In Amazon VPC, an instance does not retain its private IP.
- Security Groups operate at the instance level NOT instance level
- The default VPC CIDR is 172.31.0.0/16. Default subnets use /20 CIDRs within the default VPC CIDR.
- A default VPC can be deleted and can create a new default VPC, but old one can't be recovered

ACL's
- VPC has a default NACL with ALLOWS all inbound and outbound traffic
- Custom NACL's by default DENIES all inbound and outbound traffic
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
- Standard (order not maintained, delivered at least once) and FIFO (order retained and delivered exactly only once like banking transactions)
- default message size is 256kbps
- retention period of 14 days
- Messages are kept in the queue b/w 1 min to 14 days
- Default retention is 4 days
- SQS guarantees messages are processed at least once
- visibility timeout is b/w 30s (default)  - 12 hrs

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
- Kinesis Streams : Producers - Shards (retained for 24 hrs and can be increased up
  to 7 days) - Consumers
- Kinesis Firehose : Producers - Firehose (Analytics and takes care of consumers) - S3 - Redshift/ ELK
- Kinesis Analytics : Run SQL queries against streams/firehose
- The throughput of an Amazon Kinesis data stream scales by unit of shard. One single shard is the smallest throughput of a data stream, which provides 1MB/sec data input and 2MB/sec data output, supports up to 1000 PUT records per sec.
- number_of_shards = max (incoming_write_bandwidth_in_KB/1000, outgoing_read_bandwidth_in_KB/2000)
- Kinesis Data Streams uses an AES-GCM 256 algorithm for encryption.

Route53
- Amazon Route 53 currently supports the following DNS record types:

  A (address record)
  AAAA (IPv6 address record)
  CNAME (canonical name record)
  CAA (certification authority authorization)
  MX (mail exchange record)
  NAPTR (name authority pointer record)
  NS (name server record)
  PTR (pointer record)
  SOA (start of authority record)
  SPF (sender policy framework)
  SRV (service locator)
  TXT (text record)
  Additionally, Amazon Route 53 offers ‘Alias’ records (an Amazon Route 53-specific virtual record). Alias records are used to map resource record sets in your hosted zone to Amazon Elastic Load Balancing load balancers, Amazon CloudFront distributions, AWS Elastic Beanstalk environments, or Amazon S3 buckets that are configured as websites. Alias records work like a CNAME record in that you can map one DNS name (example.com) to another ‘target’ DNS name (elb1234.elb.amazonaws.com). They differ from a CNAME record in that they are not visible to resolvers. Resolvers only see the A record and the resulting IP address of the target record.

  EXAM TIPS
  -  EBS  can be used for web servers and workers
  -  IAM roles for tasks - Used for multi-tenant EC2 access
  -  Session Data - DynamoDB and Elastic Cache
  -  Redshift Encryption -KMS
  -  DynamoDB - most suited for metadata storage
  -  MultiValue Routing - DNS queries based routing
  -  NAT Gateway should be created in each AZ for HA.
  -  Aurora - Fully managed and MySQL / PostGre Sql COMPATIBLE
  -  NAT Gateway should always be created in public subnet
  -  Kinesis - REAL time app logs loading
  -  For Auto Scaling issues ensure right metrics are used to trigger the scale out
  -  Use Lambda for admin jobs in C#
  -  Spot Instances - for non critical applications like batch, background processing and logs
  -  Amazon Elastic search can be integrated with S3 and Lambda
  -  Docker containers are well suited for batch . Image it and deploy to ECS
  -  AWS DataSync - online data tx service that makes it faster and simpler to move data b/w on-prem storage and EFS.
  -  Is VPC peering traffic within the region encrypted - NO, but it remains private and isolated
  -  SQS FIFO can be used to queue DB writes in case of heavy write load
  -  DR - Aurora read replica
  -  Opsworks - defines stacks for different layers of the application
  -  Automation of tasks can be done with custom user data scripts in AWS while launching an instance
  -  The Public IP address of EÇ2 changes upon restart
