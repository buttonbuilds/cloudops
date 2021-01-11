# Cloud Operation Assessment

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
