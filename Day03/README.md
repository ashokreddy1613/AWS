# VPC Flow Logs

## What is VPC Flow logs?
Vpc flow logs capture the information about IP traffic going to and from network interfaces in your VPC. It is essential for auditing and tracing network traffic. Logs provide insight into ongoing traffic.

For example, if there is an breach or something, audit team may ask for VPC logs to audit and trace traffic. 

Use cases:
- Investiagte suspicious activity- see who accessed what and when
- Debug connectivity issue
- Audit - Track all ingress/engress events for audit logs

🛠️ How to Enable VPC Flow Logs
Go to VPC -> Select VPC
you will find flow logs bottom of the page

- You can choose the type of traffic to capture
- Also store logs in S3 or send to cloudWatch

For example, create a EC2 instance in VPC, and enable Traffic flow then try to hit that instance so it will capture the logs.
