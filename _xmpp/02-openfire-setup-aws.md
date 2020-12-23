---
title: "Installing Openfire on Ubuntu 18.04"
permalink: /xmpp/openfire_aws.html
excerpt: "On AWS"
header:
  overlay_image: /assets/images/xmpp/02-openfire-setup-aws/openfire_aws_ubuntu.png
  teaser: /assets/images/xmpp/02-openfire-setup-aws/openfire_aws_ubuntu.png
  overlay_filter: 0.5
last_modified_at: 2019-06-17T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---

## openfire setup on ubuntu 18.04 ec2 instance on aws

### Spin up Ubuntu Instance on AWS

1. Follow [this guide] and start a Ubuntu 18.04 instance on ec2 dashboard [launch ubuntu 18.04 instance using aws console](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html)

2. [point the domain name to the ubuntu instance using route53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-ec2-instance.html)

### openfire installation on ubuntu machine
1. [ssh into ubuntu instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
2. install java
 ```
 sudo apt update
 sudo apt install openjdk-8-jdk
 java -version
 ```
 3. openfire debian package installation
   ```
  cd /tmp
  wget https://www.igniterealtime.org/downloadServlet?filename=openfire/openfire_4.4.2_all.deb -O openfire.deb   
  sudo  dpkg -i openfire.deb 
  systemctl status openfire
   ```
4. [mysql Instalation](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04) 
 
### openfire database configuration on mysql
1. configure openfire database  
   log into mysql shell run following commands on shell  
   ```
   create database openfire;
   GRANT ALL PRIVILEGES ON openfire.* TO openfire@localhost IDENTIFIED BY 'password123!';
   flush privileges;
   use openfire;
   source /usr/share/openfire/resources/database/openfire_mysql.sql;
   exit;
   ``` 
### opening port openfire
1. [open following port on instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#adding-security-group-rule)
   9091,9090,5222,7777

### configure openfire
1. go to the [http://domain.com:9090](http://domain.com:9090)  
choose the language and continue
 ![language](/blog/assets/images/xmpp/02-openfire-setup-aws/openfire_first_page.png)
2. setup xmpp domain  
   ![xmpp domain](/blog/assets/images/xmpp/02-openfire-setup-aws/openfire_xmpp_domain.png) 
3. mysql setup  
   username: openfire  
   password: password123!  
   which we created above.  
   ![mysql](/blog/assets/images/xmpp/02-openfire-setup-aws/openfire_mysql_setup.png) 
4. continue    
   ![continue](/blog/assets/images/xmpp/02-openfire-setup-aws/openfire_database_setting.png)
5. set admin email and password  
   ![admin setup](/blog/assets/images/xmpp/02-openfire-setup-aws/openfire_admin_setup.png)
6. continue and login  
   ![login](/blog/assets/images/xmpp/02-openfire-setup-aws/openfire_login.png)



      