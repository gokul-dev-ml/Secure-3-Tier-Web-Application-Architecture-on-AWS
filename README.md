Secure 3-Tier Web Application Architecture on AWS
Overview

This project demonstrates the deployment of a secure and scalable 3-tier web application architecture on AWS using Ubuntu Linux, Nginx, Application Load Balancer, Auto Scaling, and Amazon RDS MySQL.

<img width="801" height="267" alt="Screenshot From 2026-05-28 15-11-44" src="https://github.com/user-attachments/assets/44a11926-71c6-41c3-899c-9dda24f67986" />

The infrastructure follows AWS best practices by separating the environment into public and private layers to improve security, scalability, and availability.

The architecture consists of:

Public Tier
Application Tier (Private)
Database Tier (Private)
Architecture Diagram

Architecture Components
Public Tier
Internet Gateway
Application Load Balancer (ALB)
Bastion Host
NAT Gateway
Application Tier (Private)
Ubuntu + Nginx Application Server 1
Ubuntu + Nginx Application Server 2
Auto Scaling Group
Database Tier (Private)
Amazon RDS MySQL
AWS Services Used
Service	Purpose
Amazon VPC	Network isolation and routing
Amazon EC2	Application hosting
Ubuntu Server	Operating system
Nginx	Web server
Application Load Balancer	Traffic distribution
Auto Scaling Group	High availability and scaling
Amazon RDS MySQL	Managed database
NAT Gateway	Internet access for private instances
Internet Gateway	Public internet access
Security Groups	Instance-level firewall
IAM	Secure AWS access management
Amazon CloudWatch	Monitoring and alerting
Architecture Design
VPC Configuration
Setting	Value
VPC Name	three-tier-vpc
CIDR Block	10.0.0.0/16
AWS Region	ap-south-1 (Mumbai)
Subnet Configuration
Public Subnets
Subnet Name	CIDR Block	Availability Zone
public-subnet-1	10.0.1.0/24	ap-south-1a
public-subnet-2	10.0.2.0/24	ap-south-1b
Private Application Subnets
Subnet Name	CIDR Block	Availability Zone
private-app-1	10.0.3.0/24	ap-south-1a
private-app-2	10.0.4.0/24	ap-south-1b
Private Database Subnets
Subnet Name	CIDR Block	Availability Zone
private-db-1	10.0.5.0/24	ap-south-1a
private-db-2	10.0.6.0/24	ap-south-1b
Security Architecture
Application Load Balancer Security Group

Allowed Inbound Traffic:

HTTP (80)
HTTPS (443)

Source:

Anywhere (0.0.0.0/0)
Bastion Host Security Group

Allowed Inbound Traffic:

SSH (22)

Source:

Personal Public IP Address Only
Application Server Security Group

Allowed Inbound Traffic:

HTTP from ALB Security Group
SSH from Bastion Host Security Group
Database Security Group

Allowed Inbound Traffic:

MySQL Port 3306

Source:

Application Server Security Group
EC2 Configuration
Bastion Host
Setting	Value
Operating System	Ubuntu Server
Instance Type	t2.micro
Subnet	Public Subnet
Application Servers
Setting	Value
Operating System	Ubuntu Server
Web Server	Nginx
Instance Type	t2.micro
Deployment	Private App Subnets
Ubuntu and Nginx Setup
Update System Packages
sudo apt update -y
sudo apt upgrade -y
Install Nginx
sudo apt install nginx -y
Start and Enable Nginx
sudo systemctl start nginx
sudo systemctl enable nginx
Verify Nginx Status
sudo systemctl status nginx
Create Test Web Page
echo "<h1>3 Tier Architecture Working Successfully</h1>" | sudo tee /var/www/html/index.nginx-debian.html
NAT Gateway Configuration

A NAT Gateway was deployed in the public subnet to allow private EC2 instances to securely access the internet for:

Package installation
Security updates
Software downloads

Private instances remain inaccessible directly from the internet.

Application Load Balancer Configuration
Load Balancer Details
Setting	Value
Load Balancer Type	Application Load Balancer
Scheme	Internet-facing
Listener	HTTP : 80
Target Type	Instance
Target Group Configuration
Setting	Value
Target Type	EC2 Instances
Health Check Path	/
Protocol	HTTP

Private application servers were registered with the target group and attached to the ALB listener.

Auto Scaling Configuration
Parameter	Value
Minimum Instances	2
Maximum Instances	4
Desired Capacity	2
Scaling Policy	CPU Utilization > 70%
Database Configuration
Amazon RDS MySQL
Setting	Value
Database Engine	MySQL
Deployment Type	Private
Public Access	Disabled
Multi-AZ Deployment	Enabled
Monitoring and Logging

Amazon CloudWatch was configured for:

CPU Utilization Monitoring
Instance Status Checks
Network Monitoring
Alarm Notifications
Deployment Workflow
Created custom VPC
Configured public and private subnets
Attached Internet Gateway
Configured route tables
Created NAT Gateway
Launched Bastion Host
Deployed Ubuntu EC2 application servers
Installed and configured Nginx
Created Amazon RDS MySQL database
Configured Application Load Balancer
Created Target Group
Configured Auto Scaling Group
Enabled CloudWatch monitoring
Key Features
Secure 3-tier architecture
High availability across multiple Availability Zones
Private application and database layers
Scalable infrastructure using Auto Scaling
Centralized traffic management using ALB
Secure administrative access through Bastion Host
Monitoring and alerting with CloudWatch
Challenges Faced
Configuring routing between public and private subnets
Setting up NAT Gateway for private internet access
Managing security group communication
Configuring Nginx on private EC2 instances
Troubleshooting ALB target group health checks
Establishing SSH access through Bastion Host
Future Enhancements
HTTPS configuration using AWS Certificate Manager
Domain integration with Route 53
AWS WAF implementation
CloudFront CDN integration
CI/CD pipeline using GitHub Actions
Docker containerization
Infrastructure as Code using Terraform
Kubernetes deployment using Amazon EKS
Skills Demonstrated
AWS VPC Networking
Public and Private Subnet Design
EC2 Administration
Ubuntu Linux Administration
Nginx Configuration
Amazon RDS Deployment
Load Balancer Configuration
Auto Scaling
NAT Gateway Configuration
CloudWatch Monitoring
Security Group Management
High Availability Architecture
Conclusion

This project demonstrates the implementation of a production-style AWS infrastructure using secure networking, scalable architecture principles, and cloud-native services. It provides practical hands-on experience with core AWS and DevOps concepts commonly used in real-world environments.


