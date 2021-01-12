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

## User Data
Use the following user data for the EC2 instances

```bash
#!/bin/bash
# Install Apache Web Server and PHP
yum install -y httpd mysql php
# Download Lab files
wget https://aws-tc-largeobjects.s3.amazonaws.com/AWS-TC-AcademyACF/acf-lab3-vpc/lab-app.zip
unzip lab-app.zip -d /var/www/html/
# Turn on web server
chkconfig httpd on
service httpd start
```

## Authors and Acknowledgment

* Brandon Aw for finding this User Data from the Internet
* The Internet
* Stephen Mareek
* Jon Benzo
