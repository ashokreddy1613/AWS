# Ec2 (Elastic Cloud Computing)

## What is EC2?
Aws Ec2 is a virtual server in the cloud. It allows you to run the applications on AWS infrastructure by launching instances with choice of:
 - Operating system (Linux, Windows, etc.)
 - CPU/memory/storage/network settings

## How does it works?
You deploy an web application on Ec2 instance, store file on EBS volumes, use security groups to allow HTTP/SSH, and assign an Elastic Ip or fixed public Ip.

# Components in AWS EC2

## Types of Instances

## On-Demand Instances:

Pay-as-you-go pricing model where you pay for compute capacity by the hour or second with no long-term commitments.
Ideal for short-term workloads, unpredictable usage, or testing environments.

## Reserved Instances (RIs):
Commit to a specific instance type in a region for a one- or three-year term and receive significant discounts compared to On-Demand pricing.
Suitable for steady-state workloads with predictable usage patterns, providing substantial cost savings over time.

## Spot Instances:
Bid for spare Amazon EC2 computing capacity at a significantly lower price compared to On-Demand instances.
Perfect for fault-tolerant and flexible workloads, such as batch processing, data analysis, and testing.

## Launch Templates:
Define the configuration of an EC2 instance, including the AMI, instance type, network settings, and storage, and then use it to launch instances repeatedly.
Streamlines instance provisioning and ensures consistency across deployments.

## EBS (Elastic Block store)
Amazon EBS is a block storage service used to store data for EC2 instances.
It provides durable, high-performance storage volumes that behave like physical hard drives attached to EC2

## Security
You can attach Security Groups and NACLs 

## Auto-sacling
Auto Scaling is an AWS service that automatically adds or removes EC2 instances based on demand, helping you maintain performance, availability, and cost efficiency.

## Load balancing
In AWS, EC2 Load Balancing is handled using Elastic Load Balancer (ELB), which distributes traffic evenly across multiple EC2 instances. It helps prevent any one instance from being overwhelmed and ensures seamless scaling."

By understanding the different EC2 instance types and implementing cost-saving techniques, you can effectively manage your AWS infrastructure costs while meeting your application requirements