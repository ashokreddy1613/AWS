# 🚀 AWS Interview Answers

## 🧠 General AWS Knowledge

### ⭐ How comfortable with AWS and how much do you rate yourself out of 5?
I am very comfortable with AWS and would rate myself 5/5. I have extensive experience working with various AWS services and implementing cloud solutions.

### 🔧 What are the services you have used in AWS?
I have used a wide range of AWS services including EC2, S3, VPC, IAM, Lambda, CloudWatch, Route 53, ECS, EKS, Fargate, and many more.

### 🔄 Explain how you did your cloud migration.
Our cloud migration involved a phased approach:
1. 📋 Assessment of existing infrastructure and applications
2. 📝 Planning and designing the target AWS architecture
3. 🛠️ Setting up the AWS environment (VPC, security groups, IAM roles)
4. 📦 Migrating data and applications using AWS tools like AWS Database Migration Service and AWS Server Migration Service
5. 🧪 Testing and validation of the migrated applications
6. 🔀 Cutover to the new environment
7. 📈 Post-migration optimization and monitoring

### 🔐 How do you manage AWS + Azure using a single DevOps process with focus on security & cost?
We use a multi-cloud strategy with a unified DevOps process:
- 📜 Infrastructure as Code (IaC) using Terraform to manage resources across both clouds
- 🔄 CI/CD pipelines that deploy to both AWS and Azure
- 📊 Centralized monitoring and logging using tools like CloudWatch and Azure Monitor
- 🛡️ Security policies and compliance checks implemented across both platforms
- 💰 Cost management tools to monitor and optimize spending in both clouds

### 💰 How did you perform cost optimization in AWS?
Cost optimization strategies include:
- 📏 Right-sizing EC2 instances
- 💳 Using Reserved Instances and Savings Plans
- 📈 Implementing auto-scaling
- 🎯 Using Spot Instances for non-critical workloads
- 💾 Optimizing storage (S3 lifecycle policies, EBS volume management)
- 🔍 Regular review of unused resources
- 📊 Using AWS Cost Explorer and Budgets for monitoring

### 📈 If U want to design an infra for high scalability, how did u do that?
For high scalability, we implemented:
- 🔄 Auto-scaling groups for EC2 instances
- ⚖️ Load balancers (ALB/NLB) for traffic distribution
- 🏗️ Microservices architecture
- ⚡ Serverless components (Lambda, Fargate)
- 🚀 Caching solutions (ElastiCache)
- 📊 Database scaling (RDS read replicas, DynamoDB)
- 🌐 CDN for static content delivery

## 🖥️ EC2

### 💻 What is an EC2 instance?
An EC2 (Elastic Compute Cloud) instance is a virtual server in AWS's cloud. It allows you to run applications on the AWS infrastructure.

### 🔧 How do you create an EC2 instance via Terraform?
Here's a basic example of creating an EC2 instance using Terraform:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = "example-instance"
  }
}
```

### 🔒 How do you assign a custom security group and user data in EC2 via Terraform?
```hcl
resource "aws_security_group" "example" {
  name        = "example-sg"
  description = "Example security group"
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  vpc_security_group_ids = [aws_security_group.example.id]
  user_data = <<-EOF
              #!/bin/bash
              echo "Hello, World!" > /tmp/hello.txt
              EOF
  tags = {
    Name = "example-instance"
  }
}
```

### 🔄 I have created EC2 instance A, how to create B without deleting A?
You can create a new EC2 instance B by using a different resource name in Terraform or by creating a new instance in the AWS Console without affecting instance A.

### 🔍 EC2 instance is unreachable, not a SG issue – how do you troubleshoot?
Troubleshooting steps:
### 1. Instance State and Status Checks
- ✅ Verify instance is in "Running" state
- ✅ Check system and instance status checks
- ✅ Review recent instance state changes
- ✅ Check system events for any issues

### 2. Network Configuration
- 🌐 Verify public/private IP assignment
- 🌐 Confirm subnet configuration (public/private)
- 🌐 Check route table settings
- 🌐 Verify Internet Gateway attachment (if needed)

### 3. System Logs
- 📋 Access instance system logs
- 📋 Review boot sequence
- 📋 Check for system-level errors
- 📋 Verify initialization completion

### 4. User Data Script
- 📝 Review user data script content
- 📝 Check script execution status
- 📝 Look for script-related errors
- 📝 Verify script completion

### 5. IAM and Permissions
- 🔑 Verify IAM role attachment
- 🔑 Check role permissions
- 🔑 Review permission boundaries
- 🔑 Ensure necessary AWS service access

### 6. Storage and Volumes
- 💾 Check EBS volume attachment
- 💾 Verify volume health
- 💾 Monitor disk space
- 💾 Review I/O performance

### 7. Network ACLs
- 🔒 Review subnet NACL rules
- 🔒 Verify required port access
- 🔒 Check for explicit deny rules
- 🔒 Validate both inbound/outbound rules

If none of these steps resolve the issue, you might want to consider:
Creating an AMI of the instance for backup
Launching a new instance from the AMI

### 🔐 How to login to VM with private IP?
To login to a VM with a private IP, you need to:
1. 🏰 Use a bastion host or VPN to access the private network
2. 🔑 SSH into the bastion host
3. 🎯 From the bastion host, SSH into the target VM using its private IP

### Two AWS accounts are there in same organisation. Account A has Ec2 instance and Account B has some tokens.  Need to access the tokens from Ec2 instance
## 🔧 Implementation Steps

### 1. Create IAM Role in Account B
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:GetSecretValue",
                "kms:Decrypt"
            ],
            "Resource": [
                "arn:aws:secretsmanager:region:account-B:secret:token-name-*",
                "arn:aws:kms:region:account-B:key/key-id"
            ]
        }
    ]
}
```

### 2. Configure Trust Relationship
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::account-A:role/ec2-instance-role"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

### 3. EC2 Instance Role in Account A
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::account-B:role/token-access-role"
        }
    ]
}
```

### 4. Access Tokens from EC2
```bash
# Assume the role in Account B
aws sts assume-role \
    --role-arn arn:aws:iam::account-B:role/token-access-role \
    --role-session-name "TokenAccessSession"

# Set temporary credentials
export AWS_ACCESS_KEY_ID=<temporary-access-key>
export AWS_SECRET_ACCESS_KEY=<temporary-secret-key>
export AWS_SESSION_TOKEN=<temporary-session-token>

# Access the tokens
aws secretsmanager get-secret-value --secret-id token-name
```
### I need to send a logs from EC2 to S3, create an automation part where we need to take the log file, check the CPU metrics and send an alarm through cloud watch to user's.

✅ Step 1: Create an IAM Role for EC2 with Required Permissions
    Policy Permissions:
    S3: PutObject
    CloudWatch: PutMetricData, GetMetricStatistics
    SNS: Publish
    Logs: Optional, if using CloudWatch Logs

✅ Step 2: Install and Configure CloudWatch Agent for CPU Monitoring

✅ Step 3: Create CloudWatch Alarm for CPU Usage

✅ Step 4: Create SNS Topic and Subscribe Email

✅ Step 5: Automate Log File Upload via Cron

### How would you secure an EC2 instance against unauthorized SSH
access?
1. Restrict SSH at the Network Level
🔸 Security Group (SG)
Only allow port 22 from trusted IPs
3. Disable Root Login
Avoid direct root access. Create a non-root user (ec2-user, ubuntu) with sudo access.
4. Use IAM + EC2 Instance Connect or AWS Systems Manager


## ☁️ S3

### 💾 Difference between S3 and EBS.
- S3 is object storage, while EBS is block storage
- S3 is highly durable and available, while EBS is attached to EC2 instances
- S3 is used for storing files and backups, while EBS is used for EC2 instance storage

### 🔄 How do you maintain the lifecycle of an S3 bucket?

✅ What Is S3 Lifecycle Management?
  A lifecycle policy defines when and how S3 objects should be:
  Transitioned to cheaper storage classes (e.g., Glacier)
  Expired (deleted) after a certain period
  Noncurrent versions (in versioned buckets) cleaned up

  “I usually start by analyzing the data retention and access patterns. Based on that, I create lifecycle rules which:

    Automatically transition infrequently accessed data to cheaper storage classes like S3 Standard-IA, Glacier, or Deep Archive after a set number of days.

    Expire temporary or outdated objects, for example, logs older than 90 or 180 days.

    If versioning is enabled, I also set rules to delete non-current object versions after a specific period to reduce cost.

“I use S3 lifecycle rules to automatically transition data to cheaper storage and delete old or unused files — either by prefix or tag — to optimize costs and meet data retention policies.”


### 🔒 What is the purpose of creating S3 bucket policies?
S3 bucket policies are used to:
- 🛡️ Control access to the bucket and its objects
- 👥 Define who can perform which actions
- 🔄 Set up cross-account access
- 🔐 Enforce encryption requirements

"S3 bucket policies provide a way to define access control at the bucket level for users, roles, services, and even external accounts — often used for cross-account sharing and security enforcement."

### ⚠️ An S3 bucket was made public by mistake. How do you secure and audit it?
Steps to secure and audit a public S3 bucket:
1. 🔒 Remove public access settings
2. Audit: Identify What Was Exposed and When
  Use CloudTrail to check:
    Who made the bucket public
    Any PutBucketPolicy, PutBucketAcl, or object-level ACL actions
    Use S3 Server Access Logs or CloudTrail Lake to check:
    What files were accessed and from where
3. 📜 Review and update bucket policies
4. 📝 Enable versioning
5. 🔐 Enable server-side encryption
6. 🔍 Use AWS Config to monitor bucket settings
7. 🗑️ Review and remove any public ACLs



### 📊 Storing logs in S3 bucket – how do you track IPs?
To track IP addresses accessing logs stored in an S3 bucket, you need to enable access logging and monitoring tools
1. Enable S3 Server Access Logging
  🛠 Steps:
    Go to your S3 bucket (e.g., log-storage-bucket)
    Go to Properties → Server access logging
    Enable logging and choose a target bucket (e.g., log-access-audit-bucket)
    Important: The target bucket must not be the same as the source bucket.
2. CloudTrail (for API-level tracking)
Enable AWS CloudTrail to record S3 data events
  Steps:
    Go to CloudTrail > Trails > Create trail
    Enable Data events → S3 → log-storage-bucket
    Logs go to another S3 bucket or CloudWatch for analysis


### 📤 Sending log files from EC2 to S3 – what are the steps?
Steps to send logs from EC2 to S3:
1. 🔑 Create an IAM role with S3 write permissions
2. 🔗 Attach the role to the EC2 instance
3. 📤 Use AWS CLI or SDK to upload logs to S3
4. ⏰ Set up a cron job or use CloudWatch Logs agent to automate the process

### what is the difference between the S3 bucket policies and acls?
S3 bucket policies are JSON-based permissions that control access at the bucket level, while ACLs (Access Control Lists) are legacy mechanisms that manage access at the object and bucket level using grantee permissions.

“Bucket policies are the preferred, modern way to manage access using JSON-based statements and conditions. ACLs are older and allow limited, object-level access control without much flexibility. I always recommend disabling ACLs unless there's a specific legacy requirement and enforcing access using bucket policies and IAM.”

### If a user wants to access the S3 bucket, what are the processes?
If a user wants to access an S3 bucket, the access depends on who the user is (IAM or external) and how the bucket is configured (private, cross-account, public, etc.).

“To allow a user to access an S3 bucket, I either attach an IAM policy (for same-account users), configure a bucket policy (for cross-account access), or use a pre-signed URL (for temporary access). I ensure that access is secure and follows the principle of least privilege.”


Step 1: Attach an IAM Role to EC2
  a. create a role 
    ```bash
    {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```
b. Attach the role to your EC2 instance:

 Step 2: Send a Test Log File to S3 through AWS CLI
 Step 3: Automate Upload Using Cron 

### I’m an admin but I don’t have access to the S3 bucket ?

 “Even as an admin, S3 access can be blocked by bucket policies, encryption settings, and Block Public Access. I’d review the bucket policy, check for explicit denies or conditions, use the IAM Policy Simulator, and verify if any KMS key policies or VPC/IP conditions are restricting access.”

### You have an S3 bucket at us-south-1, is it possible to access that bucket from us-east-1 ?

 Yes, you can access an S3 bucket in us-south-1 from us-east-1 — because Amazon S3 is a global service, and buckets are accessible over the network from any AWS region, unless access is explicitly restricted.

## 🕸️ VPC & Networking

### 🔗 What is VPC peering?
VPC peering is a networking connection between two VPCs that enables you to route traffic between them using private IP addresses.

### 🔒 Difference between subnet and NACL?
- Subnets are segments of a VPC's IP address range
- NACLs (Network Access Control Lists) are stateless firewalls that control traffic at the subnet level

### 🌐 Difference between NAT Gateway and Internet Gateway.
- Internet Gateway allows resources in a VPC to connect to the internet
- NAT Gateway allows resources in a private subnet to connect to the internet while maintaining their private IP

### 🔒 Can you avoid the specific port traffic using SGs?
Yes, you can use Security Groups to allow or deny traffic on specific ports.

### 🛡️ Difference between SGs and NACLs.
- Security Groups are stateful and operate at the instance level
- NACLs are stateless and operate at the subnet level

### 🔒 Where do you use firewalls, SGs, and NACLs?
- Firewalls: On-premises or in the cloud for network security
- Security Groups: For instance-level security in AWS
- NACLs: For subnet-level security in AWS

### 🛡️ What are the security parameters for EC2 in production?
Security parameters for EC2 in production include:
- 🔒 Using security groups to control traffic
- 👥 Implementing IAM roles
- 🔐 Enabling encryption for EBS volumes
- 🔄 Regular security updates and patches
- 📊 Monitoring and logging

### 🌐 Transit Gateway – what is it and why use it?
Transit Gateway is a service that enables you to connect multiple VPCs and aws to on-premises networks through a single gateway. It simplifies network architecture and reduces operational complexity.

“AWS Transit Gateway is a central network hub that simplifies VPC and on-prem connectivity in a scalable way. It replaces the complexity of VPC peering meshes with a hub-and-spoke model, supports cross-account and inter-region routing, and enables centralized control of traffic. I’d use it for hybrid cloud, multi-account setups, and secure inspection architectures.”

### 🔌 How do you connect private subnet to the internet?
To connect a private subnet to the internet, you need:
1. 🌐 A NAT Gateway in a public subnet
2. 🗺️ Route tables configured to route traffic from the private subnet to the NAT Gateway

“To connect a private subnet to the internet, I create a NAT Gateway in a public subnet and update the private subnet's route table to forward internet-bound traffic through the NAT. This allows outbound internet access while keeping the subnet private. For more control, a NAT instance can also be used, but NAT Gateway is preferred for scalability and maintenance.”

### 🔗 How do you connect from AWS to on-prem servers?
You can connect AWS to on-prem servers using:
- 🔒 VPN connection
- 🌐 AWS Direct Connect
- 🔄 Transit Gateway

### ❌ Is it possible to create NAT gateway in private subnet?
No, NAT Gateways must be created in a public subnet.

### 📊 VPC Flow Logs – how do you track the IPs hitting the VPC?
VPC Flow Logs capture information about IP traffic going to and from network interfaces in your VPC. You can analyze these logs using CloudWatch Logs or Athena.

### 🔑 Two AWS accounts – access token in B from EC2 in A – how?
To access resources in Account B from an EC2 instance in Account A:
1. 👥 Create an IAM role in Account B with the necessary permissions
2. 🔄 Assume the role from Account A using AWS STS
3. 🔑 Use the temporary credentials to access resources in Account B

### How many NAT Gateways are needed for two public & two private subnets n a single VPC? Min & max?

“For a VPC with 2 public and 2 private subnets, the minimum number of NAT Gateways required is 1. This allows both private subnets to access the internet. However, the recommended and maximum practical number is 2, placing one NAT Gateway in each Availability Zone. This ensures high availability and avoids cross-AZ data transfer costs.”


### How would you set up networking in vpc?

“To set up networking in a VPC, I define CIDR blocks, create public and private subnets across availability zones, configure route tables, Internet Gateways, NAT Gateways, and associate security groups and NACLs appropriately based on access needs.

“I follow a structured approach to designing VPC networking, ensuring separation of public and private resources, high availability using AZs, and secure internet access via IGW and NAT. I always implement IAM and security groups based on least privilege and ensure VPC flow logs and CloudWatch monitoring are enabled for visibility.”

### How Does VPC Peering Work?
VPC Peering is a networking connection between two Amazon VPCs that allows private traffic to flow between them using private IP addresses, as if they were on the same network.

It works by establishing a direct route between the VPCs, without needing VPN, NAT, or internet gateways.
1. Create a VPC Peering Connection
  One VPC owner sends a peering request to another VPC (can be in the same or different AWS accounts and regions).

  The other VPC must accept the request.

2. Update Route Tables
  After peering is active, you must manually add routes in each VPC's route table to allow traffic to flow.

3. Ensure Security Groups and NACLs Allow Traffic
  VPC peering allows the network layer connection.

  You still need security group rules that allow inbound traffic from the peer VPC’s CIDR.

### If we connect VPCs to the Transit Gateway, what will you update in the VPC Route Table?
When you connect VPCs to a Transit Gateway (TGW), you must update the VPC route tables to route traffic to the Transit Gateway for any destination CIDRs (usually other VPCs or on-prem networks).

“When I attach a VPC to a Transit Gateway, I update the route table in each VPC to point the destination CIDR of peer VPCs to the TGW ID. I also ensure the TGW route table maps the CIDRs to the correct VPC attachments, and that security groups are configured to allow traffic between them.”

### For all VPCs, will you configure the Transit Gateway attachment with CIDR range?
“No, we don’t manually configure CIDR ranges during Transit Gateway attachment. The VPC's CIDR is automatically associated with the attachment. We control routing via the Transit Gateway route table, which maps destination CIDRs to the appropriate VPC attachment. This ensures that each VPC can route traffic to the correct peer via TGW.”

### What is VPC Flow Logs and how will you track the IPs hitting the VPC?

  VPC Flow Logs is an AWS feature that captures network traffic metadata going to and from network interfaces (ENIs) in your VPC.

    It helps you:
    Monitor and troubleshoot network issues
    Audit traffic for security and compliance
    Track source and destination IPs, ports, and protocols

   Enable in VPC 
 
### How to Track IPs Hitting Your VPC
  🔹 Option 1: Search VPC Flow Logs in CloudWatch
  Use a query like this in CloudWatch Logs Insights:

“VPC Flow Logs record metadata about network traffic going in and out of network interfaces in a VPC. They help me track source and destination IPs, ports, protocols, and whether the traffic was accepted or rejected. I can view logs in CloudWatch or analyze them in S3 using Athena to trace which IPs are accessing my VPC or specific EC2 instances.”


### You need to connect your DB running in private subnet, not using NAT gateway or NAT instance or bastion host, what are the other options,

“If I can’t use a NAT Gateway, NAT instance, or bastion host, I’ll use AWS Systems Manager Session Manager to connect to a managed EC2 instance inside the VPC that can access the private DB. I can also use VPC Interface Endpoints, AWS Client VPN, or SSM port forwarding to securely connect to the database without exposing it to the public or depending on traditional network hops.”

### How does Transit Gateway work and how did you configure it?

AWS Transit Gateway is a regional network hub that acts like a highly scalable router, allowing you to interconnect multiple VPCs, AWS accounts, and on-premises networks via VPN or Direct Connect.
  Transit Gateway supports:
  VPC-to-VPC routing
  VPN-to-VPC connectivity
  Inter-region peering between TGWs

How Did I Configure Transit Gateway?
 1. Create the Transit Gateway
 2. Attach VPCs to the Transit Gateway
 3. Update VPC Route Tables
 4. Update the Transit Gateway Route Table
 
### If we have security group configured in the instance do we really need nacl.

You don’t always need a Network ACL (NACL) if Security Groups (SGs) are already configured, but NACLs provide an additional layer of subnet-level security. They’re both part of a defense-in-depth strategy.

“While Security Groups are typically enough for most use cases, NACLs provide an additional layer of subnet-level stateless filtering. I use NACLs when I need explicit deny rules, subnet isolation, or to protect against misconfigured security groups. In production, I follow a defense-in-depth approach using both SGs and NACLs when appropriate.”

### SG vs NACL
“Security Groups are stateful firewalls applied to EC2 instances that allow only ‘allow’ rules. Network ACLs are stateless filters applied at the subnet level and support both allow and deny rules. I typically use SGs for day-to-day security and NACLs for broader subnet protection or when I need to explicitly block traffic.”


## ⚙️ IAM & Security

### 👥 Difference between IAM Users and IAM Roles.

- IAM Users are identities that can be used to interact with AWS services
- IAM Roles are identities that can be assumed by users, applications, or services

"IAM Users are long-term identities intended for human users, while IAM Roles are temporary, assumable identities used by AWS services or cross-account entities. Roles provide enhanced security by offering temporary credentials, and I prefer using them in production to enforce least privilege and eliminate long-term keys."


### 📜 What are IAM policies?
IAM Policies are JSON documents that define permissions in AWS — specifying what actions are allowed or denied, on which resources, and under what conditions.

  They are attached to:
    IAM Users
    IAM Groups
    IAM Roles

### So how many types of policy, IAM policy are there? IAM policies?
  1. AWS Managed Policies
    Predefined by AWS
    Attach directly to users, groups, or roles

 2. Customer Managed Policies
    Created and managed by you
    Reusable across multiple identities

3.  Session Policies
  Temporary policies passed in AssumeRole, STS, or federation
  Exist only for the session duration

### 🔑 How do you provide AWS access to new users?
To provide AWS access to new users:
1. 👤 Create an IAM user
2. 📜 Assign appropriate permissions using IAM policies
3. 🔑 Generate access keys or set up password for console access

### 🔒 What are the best password security practices?
Best password security practices include:
- 🔐 Using strong, unique passwords
- 🔑 Enabling MFA
- 🔄 Regularly rotating passwords
- 📜 Using password policies to enforce complexity

### 🛡️ How do you implement best security policies on AWS?
Implementing best security policies on AWS involves:
- 👥 Using IAM roles and policies
- 🔐 Enabling encryption
- 🔒 Setting up security groups and NACLs
- 📊 Regular security audits and compliance checks

## ⚡ Lambda

### 🚀 What is Lambda function?
AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers.

### 💻 Can you write a Lambda file?
Yes, here's a simple Lambda function in Python:
```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Hello, World!'
    }
```

### 🎯 What did you achieve with Lambda?
With Lambda, we achieved:
- ⚡ Serverless architecture
- 📉 Reduced operational overhead
- 💰 Cost savings by paying only for compute time
- 📈 Scalability and high availability

### ⚠️ Limitations of Lambda.
Lambda limitations include:
- ⏰ Execution time limit of 15 minutes
- 💾 Memory allocation limits
- 💿 Temporary storage limits
- 🔄 Concurrency limits

### 🐳 How Lambda works with containers.
Lambda can run container images, allowing you to package your code and dependencies in a container and deploy it to Lambda.


What is lambda functions? did u used any Lambda functions? what did u acheived from that?
 prepare this question


## ⚖️ Load Balancers

### 🔄 Difference between ALB and NLB.
- ALB (Application Load Balancer) is best for HTTP/HTTPS traffic
- NLB (Network Load Balancer) is best for TCP/UDP traffic

### ⚖️ Types of AWS Load Balancers.
AWS offers three types of load balancers:
- Application Load Balancer (ALB)
- Network Load Balancer (NLB)
- Gateway Load Balancer (CLB)

### ⚖️ How does weighted routing work in Load Balancers?
Weighted routing allows you to distribute traffic to different targets based on assigned weights. This is useful for testing new application versions or gradually shifting traffic.

Where Weighted Routing Is Used:
🔹 1. Route 53 (DNS-Based Weighted Routing)
    Not a load balancer, but used for routing traffic between resources like:

      Different EC2 instances
      Load balancers in different regions
      S3 static websites
      Any IP-based endpoint
   Example:
    You define two records with the same domain (e.g., app.example.com):
    Record A → ALB in us-east-1 with weight 80
    Record B → ALB in us-west-1 with weight 20
    🎯 80% of DNS traffic is routed to us-east-1, and 20% to us-west-1.


Starting from 2023, ALB supports weighted target groups within the same listener rule.

“Weighted routing allows us to split traffic between multiple targets based on defined percentages. In Route 53, it works at the DNS level, useful for region-based routing or blue/green deployments. With ALB, weighted target groups allow canary deployments by controlling how much traffic is sent to each version of the application.”


### ⚠️ Application is down, throwing 503 – troubleshooting steps.
Troubleshooting steps for a 503 error:
1. ✅ Check if the target instances are healthy
2. ⚙️ Verify the load balancer configuration
3. 📋 Check the application logs
4. 🔒 Ensure the security groups allow traffic
5. ⚙️ Verify the target group settings

### 🏗️ Design HA backend using AWS services.
For a highly available backend, use:
- 🌐 Multiple AZs
- 📈 Auto-scaling groups
- ⚖️ Load balancers
- 📊 RDS with Multi-AZ deployment
- 🚀 ElastiCache for caching

### 🌐 GSLB Load Balancer – how does it work?
Global Server Load Balancing (GSLB) distributes traffic across multiple regions or data centers. It uses DNS to route traffic to the closest or most available server.

AWS doesn’t have a service called “GSLB” by name, but you can build GSLB functionality using:

  ✅ Route 53 + Health Checks + Multi-Region ALBs/NLBs


## 🌐 Route 53

### 🎯 Use of Route 53.
Route 53 is a scalable DNS web service that provides domain registration, DNS routing, and health checking.
1️⃣ DNS Service (Domain Name Resolution)
Translates domain names (e.g., www.example.com) into IP addresses (e.g., 192.0.2.1) so that users can access web applications.
2️⃣ Domain Registration
You can buy and manage domain names directly through Route 53.
3️⃣ Traffic Routing (Global DNS Load Balancing)
Route 53 routes users to resources based on advanced routing policies:
4️⃣ Health Checks and Failover
  Route 53 can monitor the health of endpoints using:
  HTTP/HTTPS/TCP checks
  Integrated with DNS to failover to healthy endpoints

### ⚠️ Can't configure Route 53 – why?
Common reasons for not being able to configure Route 53 include:
- 🔒 Insufficient IAM permissions
- ⚙️ Incorrect DNS settings
- 🌐 Domain not registered with Route 53

### 🔄 How to configure third-party domains (e.g., GoDaddy) with Route 53?
To configure a third-party domain with Route 53:

1. Create a Public Hosted Zone in Route 53
  ✅ This will generate 4 Name Server (NS) records automatically.
2. Copy Route 53 Name Server Records
3. Update the domain's nameservers in godaddy 
4. Create DNS Records in Route 53

“To configure a GoDaddy (or third-party) domain with Route 53, I first create a public hosted zone in Route 53, copy the AWS name servers, and update them in GoDaddy’s DNS settings. Then I create the required A, CNAME, or Alias records in Route 53 to point traffic to my AWS resources like ALB, EC2, or S3 static websites.”

### 🔍 How does DNS resolution work in Route 53?
Route 53 resolves DNS queries by routing traffic to the appropriate resources based on the routing policy configured for the domain.

DNS resolution in Route 53 is the process of translating a domain name (like www.example.com) into an IP address (like 192.0.2.1) using Route 53’s highly available, scalable DNS infrastructure.


## 🐳 ECS / EKS / Fargate

### 🚀 What is ECS, EKS, and Fargate?

- ECS (Elastic Container Service) is a container orchestration service, AWS's own container orchestration service 
- EKS (Elastic Kubernetes Service) is a managed Kubernetes service
- Fargate is a serverless option for running containers without managing servers

### 🛠️ How did you set up ECS using EC2?
To set up ECS using EC2:
1. 🏗️ Create an ECS cluster
2. 💻 Launch EC2 instances into the cluster
3. 📝 Define task definitions and services
4. 🚀 Deploy applications using ECS

### 👥 What are node groups in AWS EKS?
Node groups in EKS are managed groups of EC2 instances that run Kubernetes nodes. They simplify the management of nodes in an EKS cluster.

### ⚙️ How do you configure EKS node groups?
To configure EKS node groups:
1. 🏗️ Create a node group in the EKS console or using AWS CLI
2. ⚙️ Specify the instance type, AMI, and other configurations
3. 📈 Set up auto-scaling policies

“To configure EKS node groups, I create either a managed or self-managed group and associate it with private subnets and an IAM role. For managed groups, AWS handles provisioning and lifecycle. I ensure the right policies are attached, the node group is linked to the cluster

### 🔄 How to upgrade EKS cluster?
To upgrade an EKS cluster:
1. Check Current Version and Plan Upgrade
2. Upgrade the EKS Control Plane
3. Upgrade Managed Node Groups
4. Upgrade Kubernetes Add-ons (Important!)
5. Validate Cluster Upgrade


### 📈 Cluster auto-scaling – how do you configure it?
To configure cluster auto-scaling in EKS:
1. ⚙️ Install the Cluster Autoscaler
2. 📊 Configure auto-scaling policies for node groups
3. ⚠️ Set up CloudWatch alarms for scaling events

### 🌐 Purpose of using CNI in Kubernetes.
The Container Network Interface (CNI) in Kubernetes is used to configure networking for pods. It allows for flexible and efficient network management.
CNI (Container Network Interface) is used in Kubernetes to provide networking to pods, enabling them to communicate with each other, with services, and outside the cluster.

In AWS EKS (default): AWS VPC CNI
  Each pod is assigned an ENI (Elastic Network Interface) and an IP from the VPC subnet
  Pods are first-class citizens in the VPC network
  Allows direct communication with AWS services, security groups, etc.

### 🚀 Did you run workloads on Fargate?
Yes, we ran workloads on Fargate to eliminate the need to manage servers and simplify container deployment.

## 📊 CloudWatch & Monitoring

### 📈 What is CloudWatch used for?
CloudWatch is used for monitoring AWS resources and applications. It collects metrics, logs, and events, and can trigger alarms and automated actions.

### ⚙️ How do you collect CPU metrics in EC2 and send alarms?
To collect CPU metrics in EC2 and send alarms:
1. 📊 Enable detailed monitoring for EC2 instances
2. ⚠️ Create CloudWatch alarms based on CPU utilization
3. 📢 Configure actions for alarms (e.g., sending notifications)

### 🔍 How to filter a particular IP from AWS CloudWatch Logs?
You can filter CloudWatch Logs using metric filters or by writing custom queries using CloudWatch Logs Insights.

### In CloudWatch, what is the use of log groups and log trails?
“In CloudWatch, log groups are used to store and organize logs from AWS services like EC2, Lambda, and ECS. You can apply retention, metric filters, and subscriptions to log groups. CloudTrail, on the other hand, records API activity for auditing and can optionally send logs to CloudWatch for real-time monitoring and alerts.”

### 📊 What kind of monitoring did you set up for CPU/Disk in AWS?
We set up monitoring for CPU and disk usage using CloudWatch metrics and alarms. This included:
- 💻 CPU utilization
- 💾 Disk read/write operations
- 💿 Disk space usage

### 📢 How do you receive alerts in your project?
We receive alerts through:
- 📱 SNS notifications
- 📧 Email
- 💬 Slack integration
- 📟 PagerDuty

## 💾 Storage & Backup

### 💾 How do you take backup of AWS services?
We take backups of AWS services using:
- 💾 AWS Backup for centralized backup management
- 📸 Manual snapshots for EBS volumes
- 🔄 S3 lifecycle policies for data archiving

### 📁 Where do you store logs after backup is created?
Logs are stored in S3 buckets with appropriate lifecycle policies to manage retention and costs.

## 🛠️ Terraform with AWS

### 📝 Write Terraform code to create EC2, S3, VPC.
Here's a basic example:
```hcl
resource "aws_vpc" "example" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "example-vpc"
  }
}

resource "aws_subnet" "example" {
  vpc_id     = aws_vpc.example.id
  cidr_block = "10.0.1.0/24"
  tags = {
    Name = "example-subnet"
  }
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.example.id
  tags = {
    Name = "example-instance"
  }
}

resource "aws_s3_bucket" "example" {
  bucket = "example-bucket"
  tags = {
    Name = "example-bucket"
  }
}
```

### 🔄 How do you manage Terraform state in AWS (local vs remote)?
We manage Terraform state remotely using S3 and DynamoDB for state locking. This ensures state consistency and allows for team collaboration.

### ⚠️ What happens if Terraform state file is lost?
If the Terraform state file is lost, you may lose track of the resources managed by Terraform. It's crucial to use remote state storage and backups to prevent this.

### 🔄 How do you migrate backend to S3 with DynamoDB locking?
To migrate the Terraform backend to S3 with DynamoDB locking:
1. 💾 Create an S3 bucket and DynamoDB table
2. ⚙️ Configure the backend in your Terraform configuration
3. 🚀 Initialize Terraform with the new backend

### 📄 What is backend.tf – not visible in storage account?
The `backend.tf` file is used to configure the Terraform backend. If it's not visible in the storage account, ensure that the file is correctly named and located in your Terraform project directory. 