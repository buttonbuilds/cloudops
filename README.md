# Cloud Operation Assessment

## VPC
Create a VPC with the following IPv4 CIDR Range - 192.168.0.0/16

## Subnets
Subnet Name | Subnet CIDR block | Availability Zone 
--- | --- | --- 
Public subnet web-tier-1 | 192.168.0.0/24 | ap-southeast-1a
Private subnet app-tier-1 | 192.168.2.0/24 | ap-southeast-1a
Private subnet db-tier-1 | 192.168.4.0/24 | ap-southeast-1a
Public subnet web-tier-2 | 192.168.1.0/24 | ap-southeast-1b
Private subnet app-tier-2 | 192.168.3.0/24 | ap-southeast-1b
Private subnet db-tier-2 | 192.168.5.0/24 | ap-southeast-1b

## Create an Internet Gateway
You would have to select the newly created IGW and attach it to your VPC

![alt text](https://github.com/buttonbuilds/cloudops/blob/main/images/igw.png?raw=true)

## Route Tables
### Public Route Table
Destination | Target
--- | ---
192.168.0.0/16 | local
0.0.0.0 | \<igw\>
  
### Private Route Table
Destination | Target
--- | ---
192.168.0.0/16 | local

## Load Balancers
Name | Scheme | AZ1 | Subnet | AZ2 | Subnet
--- | --- | --- | --- | --- | ---
ext-lb | internet-facing | ap-southeast-1a | Public subnet web-tier-1 | ap-southeast-1b | Public subnet web-tier-2
int-lb | internal | ap-southeast-1a | Private subnet app-tier-1 | ap-southeast-1b | Private subnet app-tier-2

## User Data
Use the following user data for the EC2 instances

```bash
#!/bin/bash

sudo su

yum update -yum
yum install -y httpd.x86_64
systemctl start httpd.service
systemctl enable httpd.service
az=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone)

echo "Hello World from $(hostname -f) residing in $az" > /var/www/html/index.html
```
## Security Groups
SG Name | Type | Port Range | Source
--- | --- | --- | --- 
sg-ext-lb | HTTP | 80 | 0.0.0.0/0
sg-web-tier | HTTP | 80 | sg-ext-lb
"" | SSH | 22 | \<My IP\>
sg-int-lb | HTTP | 80 | sg-web-tier
"" | ICMP | | sg-web-tier
sg-app-tier | HTTP | 80 | sg-int-lb
"" | ICMP | | sg-int-lb
sg-rds-tier | Custom TCP | 3306 | sg-app-tier

## Authors and Acknowledgment

* Everyone in TFIP programme for the constant support
