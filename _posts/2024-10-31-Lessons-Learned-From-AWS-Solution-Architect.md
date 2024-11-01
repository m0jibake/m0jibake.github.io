Here's a collections of insights and learnings I gathered while I studied for the AWS Solution Architect Associate.

# AWS Services and Features Overview

## Amazon FSx
### Amazon FSx for ONTAP
- Provides feature-rich, fast, and flexible shared file storage accessible from Linux, Windows, and macOS.
- Supports asynchronous data replication across Availability Zones and AWS Regions using NetApp SnapMirror.
- On-demand data replication is primarily used by Amazon FSx for OpenZFS.

## DynamoDB
### Point-in-Time Recovery (PITR)
- Allows continuous backups of tables with restoration to any specific point in time within a retention period of up to 35 days.

### Transactions
- Groups a series of operations into a single transaction, ensuring ACID compliance for applications like financial transactions.

### Backup Limitations
- On-demand backups cannot be copied to different accounts or Regions. Use AWS Backup for advanced features.

## AWS CloudTrail
### Log File Integrity Validation
- Determine if a log file was modified, deleted, or unchanged after delivery.

## Amazon Aurora
### Auto Scaling
- Adjusts the number of Aurora Replicas to improve read capacity when Auto Scaling is enabled.

### Custom Endpoints
- Provides load-balanced database connections based on criteria other than read/write capability.

## Amazon S3
### Server Access Logging
- Offers detailed logging beyond CloudTrail.

### Batch Replication
- Supports replicating existing objects before replication configuration, using Batch Operations. 
- Differences noted between batch replication and live replication.

## AWS Proton
### Deployment
- Enables efficient deployment of serverless or container-based applications with infrastructure standards and continuous delivery pipelines.

## AWS Transfer Family
### File Transfer
- Secure file transfer over SFTP, FTPS, and FTP into/out of Amazon S3 or Amazon EFS.

## Networking
### Gateway Load Balancer
- Used to deploy and scale third-party virtual appliances (firewalls, intrusion detectors).

### Network Load Balancer (NLB)
- Operates at Layer 4, routing based on IP protocol data, and exposes a fixed IP for predictable access.

### Application Load Balancer (ALB)
- Supports cross-zone load balancing (enabled by default) and routing for gRPC traffic.

## Amazon Kinesis
### Data Streams
- Recommended over SQS for multiple applications consuming the same stream concurrently.

### Enhanced Fan-out
- Provides individual throughput for consumers in parallel streams.

## Amazon CloudFront
### Field-Level Encryption
- Allows secure uploads of sensitive information.

### File Size Limitation
- Works with files smaller than 1 GB for uploads/downloads.

## AWS Snowball
### Data Transfer
- Snowcone devices come pre-installed with the DataSync agent for online data transfer tasks.

## Security and Monitoring
### AWS Security Hub
- Offers a comprehensive view of security posture across AWS services.

### CloudWatch vs. Enhanced Monitoring
- CloudWatch gathers metrics from the hypervisor; Enhanced Monitoring (RDS only) gathers metrics from an instance agent.

## EBS and EC2
### EBS Volumes
- Support live configuration changes (volume type, size, IOPS) without interruptions.

### Auto Scaling
- Rebalances instances across Availability Zones without compromising performance.

## Redis
### Geospatial Data
- Provides commands for operations like distance calculation and finding elements within a given distance.

## Miscellaneous
### Cross-Origin Resource Sharing (CORS)
- Allows client web applications from one domain to interact with resources in another domain.

### TLS Certificates
- Multiple TLS secured applications can be hosted behind a single load balancer using Server Name Indication (SNI).

### RAID Configurations
- Use RAID 1 for fault tolerance over I/O performance.

### AWS Firewall Manager
- Centrally configures WAF rules and security measures across accounts, excluding Network ACLs.

### Instance Profiles
- Required for storing roles used with EC2 or other services leveraging EC2.

### IP Addressing
- IPv4 is the default for VPCs; subnets cannot span multiple Availability Zones.

# Networking and VPC

## VPC Peering
- A VPC peering connection allows traffic between two VPCs using private IPv4 or IPv6 addresses, enabling instances in either VPC to communicate as if on the same network.

## VPC Sharing (Resource Access Manager)
- Allows multiple AWS accounts to share resources in a central VPC. The VPC owner shares subnets with participant accounts within the same AWS Organization. Participants can create, modify, and delete their resources but can't manage others' resources.

## VPC Endpoints
- **Interface Endpoint**: Elastic Network Interface with a private IP from your subnet's range, serving as an entry point to a supported service.
- **Gateway Endpoint**: Used for routing traffic to supported services (Amazon S3 and DynamoDB) in your route table.

## NAT Instance
- A NAT instance in a public subnet enables instances in a private subnet to initiate outbound IPv4 traffic to the Internet while blocking inbound initiated traffic.

## AWS Transit Gateway
- A network transit hub that interconnects your VPCs and on-premises networks, allowing for scaling of IPsec VPN throughput with equal-cost multi-path (ECMP) routing support over multiple VPN tunnels.

## AWS VPN
- **Site-to-Site VPN**: A secure VPN connection between your on-premises networks and AWS.
- **VPN CloudHub**: Enables communication between multiple remote sites through the AWS Site-to-Site VPN connections or AWS Direct Connect.

# Security and Access Management

## Security Groups
- All inbound traffic is blocked by default.
- All outbound traffic is allowed by default.
- **Common Port Uses**:
  - 22: SSH (Linux login, TCP protocol)
  - 21: FTP (file upload, TCP protocol)
  - 22: SFTP (secure file upload)
  - 80: HTTP (unsecured websites)
  - 443: HTTPS (secured websites)
  - 3389: RDP (Windows login)

## AWS GuardDuty
- Analyzes AWS CloudTrail events, VPC Flow Logs, and DNS logs using machine learning and integrated threat intelligence to detect threats.
- Integrates with Amazon EventBridge for actionable alerts.
- **Suspension Options**:
  - **Disabling**: Deletes findings and configurations.
  - **Suspending**: Stops analysis but retains findings and configurations.

## AWS Lambda
- Encrypts environment variables by default; recommended to use a custom KMS key for additional security.
- **Concurrency Limit**: Supports 1000 concurrent executions per AWS account per region. If exceeded, throttling occurs, and a limit increase requires contacting AWS support.
- **Layers**: Allows adding dependencies and libraries as ZIP archives.
- **Function URLs**: HTTP(S) endpoints dedicated to your Lambda function.

## AWS Key Management Service (KMS)
- **Logging**: Usage of KMS keys is logged in CloudTrail.
- **Key Deletion**: Deleting a KMS key is destructive. A waiting period from 7 to 30 days (default 30) is enforced before permanent deletion, during which it can be canceled.
- **Custom Key Store**: Combines AWS CloudHSM controls with AWS KMS integration, allowing dedicated key storage with CloudHSM.
- **Key Conversion**: You cannot convert an existing single-Region key to a multi-Region key.

## IAM Roles
- IAM Roles can't be assigned to groups; only IAM policies can be assigned to groups. IAM permission boundaries can only be applied to roles or users, not IAM groups.

# Storage and Data Access

## Amazon S3
- **Transition Rules**: Objects must be stored for a minimum of 30 days in S3 Standard before transitioning to S3 One Zone-IA. S3 Standard-IA and One Zone-IA have a minimum storage duration of 30 days, while Glacier Deep Archive has a minimum of 180 days.
- **Access Points**: Simplify access by providing named endpoints with custom permissions and network controls.
- **Multi-Region Access Points**: Routes traffic through AWS Global Accelerator for optimal performance.
- **Object Lock**:
  - **Governance Mode**: Users cannot delete or modify unless granted special permissions.
  - **Compliance Mode**: Prevents all users, including root, from deleting or modifying an object for a set retention period.
  - **Legal Hold**: Protects objects until explicitly removed.
  - **Retention Period**: Locks object versions until the expiration date.
- **Multipart Upload**: Recommended for objects over 100 MB.
- **Cross-Region Replication**: Takes up to 15 minutes to replicate across regions.
- **Object Lock Requirement**: Enabled only on bucket creation, with versioning enabled.

## Amazon EFS vs. S3
- EFS does not expire files, while S3 offers options for file expiration.

## AWS Storage Gateway
- **File Gateway**: Functions as a file system mount on S3, supporting NFS and SMB protocols.
- **Tape Gateway**: Not immediately retrievable; integrates with Glacier Deep Archive.

## DataSync
- Works on-premises and can copy data between NFS, SMB file servers, self-managed object storage, AWS Snowcone, S3 buckets, EFS file systems, and FSx for Windows File Server file systems.

# AWS Global Accelerator
- A network layer service that improves availability and performance by directing traffic to optimal endpoints via the AWS global network.
- Provides two static anycast IP addresses as fixed entry points to application endpoints.
- Routes traffic across regions (unlike ELB, which operates within a single region).
- Good for non-HTTP use cases requiring static IPs or fast regional failover.

# Content Delivery and Load Balancing

## CloudFront
- **Signed URLs**: Use for individual file access control (e.g., application downloads or RTMP distributions).
- **Signed Cookies**: Control access to multiple files (e.g., video playlists or subscriber areas) without changing URLs.

## ELB vs. Global Accelerator
- ELB provides load balancing within one region, whereas AWS Global Accelerator provides traffic management across multiple regions.
- **S3 Access Points**: Restrict access to specific VPCs. S3 Multi-Region Access Points use Global Accelerator.

# Compute

## AWS Lambda
- **User Data**: Scripts entered as user data run as root and only during the first boot cycle of an instance.
- **Spot Blocks**: Available only for up to 6 hours; AWS has stopped offering Spot blocks to new customers.
- **Launch Configurations**: Immutable once created. Once you have created a launch template, it can’t be modified.

# Event Management

## Amazon CloudWatch Events
- Can trigger Lambda, SNS, or ECS tasks, enabling automation and alerting.

## Step vs. Simple Scaling Policies
- **Step Scaling**: Adjusts based on the size of an alarm breach.
- **Simple Scaling**: Adds or removes instances based on a single threshold.

# Additional Notes

## Enterprise Identity Federation
- Enables Single Sign-On (SSO), allowing users to access AWS without creating new AWS identities.

## Amazon Comprehend Medical
- A natural language processing service that makes it easy to use machine learning to extract relevant medical information from unstructured text.

## Amazon Pinpoint Event Stream
- Includes information about user interactions with applications connected to Amazon Pinpoint.

## AWS Health Events
- Generated for ACM certificates that are eligible for renewal under two scenarios: successful renewal of a certificate or when action must be taken for a renewal to occur.

## Redis AUTH
- Improves security by requiring a password for accessing a Redis server.

## Amazon SQS
- FIFO queues support up to 3,000 messages per second when batching 10 messages per operation. By default, FIFO queues support up to 3,000 messages per second with batching or up to 300 messages per second without batching.
- Messages remain in the queue until deleted; must be explicitly deleted after processing to avoid reprocessing.
- **Retention period** configurable from 1 minute to 14 days (default is 4 days).

## AWS Systems Manager Run Command
- Lets you remotely and securely manage the configuration of your managed instances.

## Karpenter
- A Kubernetes auto-scaler.

## AWS X-Ray
- Used for microservice debugging and analysis.

## AWS Network Firewall
- Can be used to monitor and analyze traffic at the VPC level. However, it cannot be directly integrated with the Application Load Balancer (ALB); AWS WAF can be integrated with ALB.

## Network Access Analyzer
- A feature of VPC that reports on unintended access to your AWS resources based on the security and compliance settings, but it does not perform deep packet inspection.

## Egress-Only Internet Gateway
- A VPC component that allows outbound communication over IPv6 from instances in your VPC, preventing the Internet from initiating an IPv6 connection with your instances.

## Elastic Fabric Adapter (EFA)
- An Elastic Network Adapter (ENA) with added OS-bypass functionality. OS-bypass capabilities are not supported on Windows instances.

## Amazon Aurora Failover
- When failing over, Amazon Aurora flips the canonical name record (CNAME) for your DB Instance to point to a healthy replica. If no replica exists, Aurora attempts to create a new DB instance in the same Availability Zone.

## DNS Record Types
- **A**: Maps a hostname to IPv4.
- **AAAA**: Maps a hostname to IPv6.
- **CNAME**: Maps a hostname to another hostname. Can’t create a CNAME record for the top node.
