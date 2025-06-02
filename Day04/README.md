# VPC Endpoint in AWS
A VPC endpoint allows private connections between your VPC and AWS services like S3, DynamoDB etc without using:
    - The public internet
    - NAT Gateway
    - VPN
    - Transit Gateway

It enables secure, fast, and cost-effective access to AWS services over the AWS private network.

Imagine a scenario where you have deployed highly secure application within a VPC in your AWS account, this application needs  to securly access AWS S3 without exposing it to public internet. You can use Endpoints to eshtablish the connection.

Types of Endpoints:
    1. Interface Endpoint
        Elastic Network Interface (ENI) in your subnet that connects to AWS service (e.g., SSM, Secrets Manager, EC2 API)
    2. Gateway Endpoint
        Route table entry for S3 or DynamoDB — no ENI involved

# 🧱 Real-World Examples
1. 🔒 Accessing S3 from a Private Subnet Without NAT Gateway

Problem:
You're in a private subnet and want to access S3. Normally, you'd need a NAT Gateway.

Solution:
Use a Gateway VPC Endpoint for S3:
    - No NAT
    - No internet
    - Still can read/write S3 securely

# 🛠️ How to Create a VPC Endpoint (AWS Console)

✅ Gateway Endpoint (S3, DynamoDB)
1.Go to VPC → Endpoints → Create Endpoint

2.Choose Service category: AWS services

3.Service name: e.g., com.amazonaws.<region>.s3

4. Type: Gateway

5.Select VPC and route table

6.Create

➡️ Adds route like: S3 → vpce-xxxxxx

