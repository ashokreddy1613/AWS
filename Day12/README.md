#IAM
IAM- Identity and Access Management

IAM is used to control authentication and autorization in AWS. It allows to define users, roles,  and permissions using policies to securely manage access to AWS services and resources.

In other words, IAM manage **who what can DO**

## Key Concepts
1. User- An IAM user is an identity with long-term credentils or access (via username/password or access keys).
for example, you create **IAM user** for one your developer in your organization and grant read-only permission to any service

2. Role
A role is a identity with temporary access to entity like AWS service, cross aws account

For example, Allow EC2 to Access s3 
if you want your ec2 to upload or download log/ files to s3 bucket securely
🛠️ Solution:
- Create an IAM role with an S3 policy (e.g., s3:PutObject, s3:GetObject)
- Attach the role to the EC2 instance
- Application uses Instance Metadata Service (IMDS) to get temporary credentials

3. Group
A group is a collection of users who can share the sam permissions
Suppose, there number of developers or users in your organization, you cant create role every time so you a create a group with policies then you add those users to the group.

4. Policy
JSON document that defines permissions (what actions are allowed or denied)


## IAM Identity Center 
It is a cloud-based solution that provides single sign-on (SSO) and centralized access management for multiple AWS accounts and business applications. It supports integration with Microsoft AD, Okta, Azure AD, and others.

IAM Identity Center is AWS’s centralized service for managing workforce access to multiple AWS accounts and applications — all from one login.

It simplifies user and access management by integrating with your corporate identity provider or letting you manage users directly within AWS.

