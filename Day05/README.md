# Security Groups vs NACLS
Both Security Groups and NACLs use to control the traffic but they serve at different level,

Let's dive to understand SGs and NACLs

# Security Groups
SGs are stateful firewalls, they act at the instance level based on rules, SGs are associated with individual instances

## Real-time Example:

Lets say you have an instance 
By default, All inbound traffic is denied and all bound traffic is allowed. Now, if you wanna connect to that instance you should allow traffic. Example, if you allow port 80, the return traffice is automatically allowed.

## ✅ Use Security Groups for:

Allowing HTTP (80), HTTPS (443), SSH (22) to EC2
Controlling which app can talk to the database (e.g., allow port 3306 from app SG)

# NACls
NACLs, on the other hand, act as stateless firewall, controlling traffic at the subnet level. 

## Real-time Example
Let's consider a scenario where you have a web server that needs to be accessible from the internet. Here's the setup:

Outbound Rules: Allow all traffic.
Inbound Rules: Allow TCP port 80 from 0.0.0.0/0 (anywhere) for web traffic and TCP port 22 from your IP address for SSH access.

By configuring NACLs in this manner, you ensure that web traffic (HTTP) is allowed from anywhere while SSH access is restricted to your IP address only.

- You must allow both inbound and outbound rules explicitly for traffic to succeed.

## ✅ Use NACLs for:
- Blocking a specific IP range at subnet level
- Allowing or denying a known CIDR range across all resources