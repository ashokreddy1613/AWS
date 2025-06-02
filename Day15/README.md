# AWS Config

## What is AWS Config?
AWS config is a governance, compliance, auditing service. It continously monitors, records, evaluate the configuration of your AWS resources- helping you detect drift, enforce compliance and view hostorical changes.

In other words, Aws config, continously track changes to AWS resources and evaluates against desired configurations.

Let me it in simple way:

As a senior devops engineer, you want to use certain EC2(t3.medium) or s3 bucket should be encrytped.

if someone in your organizatiom creates EC2 with t3.large or s3 bucket is unencrypted AWS config send non-compliance alerts via SNS.

- It shows who changed what and when 
- Integrates with CloudTrail, SNS, AWS Organizations, Lambda, Security Hub
- Track Security Group Drift
    Detect when a security group opens port 22 to 0.0.0.0/0

## How It Works

1. AWS Config records resource changes (e.g., EC2, S3, SGs)
2. Evaluates changes against rules (managed/custom)
3. Sends alerts for non-compliance
4. Optionally triggers remediation (e.g., Lambda fix)