# AWS Storage

AWs provide wide range of storage services optimized for different purposes like Block storage, Object storage, file syytems, backups, archival, etc.

## EBS (Elastic Block Storage)
AWS EBS is a block level storage service for Ec2. It provides persistent storage that can be attact to any EC2 instance, similar to hard drive, and used to store data like OS, applications, database.

Key Features:

- Persistent: Data stays even after EC2 stop/terminate (unless deleted)
- Attachable: Attach/detach to/from EC2 (within same AZ)
- Snapshots:  Create backups to S3
- Encryption: Supports encryption at rest and in transit
- Resize Anytime: Increase size or change volume type dynamically

When an EC2 instance is stopped, the attached EBS volume remains and retains its data. When the instance is terminated, the root EBS volume is deleted by default, unless we set DeleteOnTermination = false. Additional EBS volumes are retained unless configured otherwise."

## Object Storage
Object storage in AWS is provided by Amazon S3, which stores unstructured data as objects in buckets. It's ideal for scalable, durable, and cost-effective storage of data like images, videos, logs, and backups. Offers high durability, availability, and scalability at a low cost.

## File system
Fully managed file storage service that supports NFSv4 protocol. Offers scalable and highly available file storage for Linux-based workloads, allowing multiple EC2 instances to access the same file system concurrently. AWS FSx: Provides fully managed file systems optimized for Windows-based workloads, including Windows File Server and Lustre.

## AWS Glacier
Amazon S3 Glacier is a Low-cost, long-term archival storage. Ideal for backups and compliance data.

## AWS backup 
Centralized backup service for EC2, RDS, EFS, DynamoDB, etc.