# S3 
Amazon S3 (simple storage service) is an object storage service that let you store and retrive any kind of data at scale.

You can create a bucket and store data like logs, backups , store build artifacts, static website content, large data files.

It offers:
1. Scalablity
2. Highlt available
3. Secure and cost effective
4. Integrate with nearly every AWS service

- It's a globally access service
- Aws create replica of your data in different zones so reliability is very high.

## Scalability
Store almost unlimited data in a single bucket. However, one object shouldn't be morethan 5TB

## Security
S3 provides bucket policies, access control, and encryptions 
Encrypt data at rest using server side encrption
Encryption in transit by using SSl/TLS for data transfer

## Cost Effective
Cheap, ofcourse it depends on the storage class that you use.

## 📦 Core Concepts

1. Bucket- A container for storing objects (must have unique name)
2. Object- A file stored in S3 (data + metadata + unique key)
3. Key-    The unique path or name of an object in a bucket
4. Region-	S3 bucket resides in a specific AWS region
5. Storage Class-	Tier that defines cost/performance/durability
6. Versioning-	Stores multiple versions of an object
7. ACL / Bucket Policy-	Control who can access what

- Bucket Policies	
Control access at the bucket level
- ACLs	
Legacy access control at object level

Usecase: Store EC2 Logs in S3 for Centralized Access & Backup


## lifecycle

A lifecycle policy defines when and how S3 objects should be: Transitioned to cheaper storage classes (e.g., Glacier) Expired (deleted) after a certain period Noncurrent versions (in versioned buckets) cleaned up

“I usually start by analyzing the data retention and access patterns. Based on that, I create lifecycle rules which: Automatically transition infrequently accessed data to cheaper storage classes like S3 Standard-IA, Glacier, or Deep Archive after a set number of days. Expire temporary or outdated objects, for example, logs older than 90 or 180 days. If versioning is enabled, I also set rules to delete non-current object versions after a specific period to reduce cost.