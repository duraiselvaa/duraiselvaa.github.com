---
layout: post
title: "CI Part1. Bootstraping EC2 Linux Instance"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## Amazon AWS

[Amazon](http://aws.amazon.com/) have been the front runner in providing Cloud Computing Services. I have been fascinated by their continous innovation in this sphere. Here in this blog I would go through step-by-step guide in setting up a Linux Instance.

```
I don't want to clutter screenshots in this blog page. But for illustration purpose, I have embedded them in a link wherever relevent.
```

If you have a login already for AWS then go to [AWS Management Console](https://console.aws.amazon.com/console/home) or continue with [Sign Up](https://aws-portal.amazon.com/gp/aws/developer/registration/index.html) as shown - [Image: Signin](/images/aws_signin.png). Sign up using personal information and payment details. There will be an automated call to the phone number we have entered to verify the identity. Enter the PIN number specified in the verification page to complete the identity verification.
Once the identity is verified, the account will be activated. Now we can sign in and start using AWS.

## Free Tier Usage

AWS allows free usage of its services for beginners. The details of the configuration and usage limit for this could be found at [Free Tier](http://aws.amazon.com/free/)

## Creating EC2 Instance

After signing in we can see all the AWS services listed in the home page as shown in [AWS Console](/images/aws_console_home.png). Click on EC2, to view the [EC2 Console Dashboard](/images/ec2_home.png) Please check the region where you want to run your services. Its usually available to select from a list on the top-left corner. Click `Launch Instance`, and in the resulting wizard, choose the [Launch Classic Wizard](/images/ec2_launch_instance.png) option. Click `Continue`

[CHOOSE AN AMI](/images/ec2_choose_ami.png) Choose the AMI `Ubuntu Server 11.10` - 32 bit.

[INSTANCE DETAILS](/images/ec2_instance_details.png) Once the AMI is selected, the wizard will take us to the `Instance Details` page, where we can choose the number of instances and the type. Leave the `Number of Instances` as 1 and select the Instance type as `Micro (t1.micro, 613 MB)` - this is the Free tier eligible. And choose the `Launch Instances` option (not `Request Spot Instances`). Under the `Launch Instances` we can specify the `Availability Zone`  - The default option `No Preference` should be ok. In the next page we will get to choose `Advanced Instance options`. Leave all the options to default settings and click `Continue`. In the [next page](/images/ec2_key_value.png), add a key and value of your choice.

[CREATE KEY PAIR](/images/ec2_create_key_pair.png) In the next page, we could choose an existing key pair, or create and download a new one. This is used to securely connect to this instance once its launched. Give a name and click `Create and Download your Key Pair`, save the key pair in your local machine - `rest-server-key.pem`. Then click `Continue`

[CONFIGURE FIREWALL](/images/ec2_configure_firewall.png) In the next page, select an already existing security group to configure the firewall. We could also create a new security group at this point, if we don't have one already. Port 22 is by default open. If we want to add new port - say 80 for a http server, select inbound rules as `Custom TCP rule` and port range as `80` and leave source as `0.0.0.0/0` which means that accept inbound from any machines.

[REVIEW](/images/ec2_create_review.png) Next is the Review page, where we will get to see all the detials, and would be able to edit them if necessary. Make sure that all the details are correct before clicking `Launch` button.

## EC2 Listing

Once the instance is launched, it will be listed in the [Instances](/images/ec2_instances.png) page.

## Elastic IP

Our Ubuntu instance is given a random public IP during each start & stop. In order to associate a fixed IP, we can allocate an elastic IP. The Elastic IP is meant to preserve a single IP throughout the lifetime of the instance. To do association, select the left hand menu `Elastic IPs` under `NETWORK & SECURITY` and click `Allocate New Address` - this will allocate the IP address. Then select the new IP and then click `Associate Address` - a new popup will show the list of EC2 instances that are available. Please select the one we recently created. [Illustration](/images/ec2_elastic_ip.png)

***
***
<br><br>
## Remote login - SSH

When the instance is created, the `.pem` file should be saved from the AWS console

## Connect from Windows
As there is no native ssh utility in windows, use putty to connect to ec2 instance, use `puttygen` and `putty` utilities from [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

*1. Use `puttygen` to generate another format of the private key*

- load the existing `pem` private key (**note**: in the browse dialog, make sure the filter set to `All Files` as illustrated [here](/images/puttygen_load_key.png)
- save private key (in _ppk_ format). You will be asked 'if you want to save this private key without a passphrase'. you may choose a passphrase, and save the private key. You will be asked to enter a name for the key in ppk format and then the key in .ppk format will be saved.

*2. Configure the connection in `putty`*

- Session: Host Name (or IP address): the elastic IP associated with the instance
- Setting the newly generated private key - Browse to `Connection - SSH - Auth` Browse for the private key (in ppk format)
- then save the profile for future use. [Illustration](/images/putty_connect.png)
- user (login as)= `ubuntu` (when the connection is established)

<br>
## Connect from Linux
`ssh` command is used with the `.pem` format for the key

	ssh -i rest-server-key.pem ubuntu@dns_of_machine

where `ubuntu` is the default admin user for the EC2 instance; `dns_of_machine` will be your elastic ip.


***
***
<br><br>

## Installing JAVA, MySQL, Tomcat

<br><br>
***
<br><br>

## Update the system and install few of the needed tools
<br>

	sudo apt-get -y -qq update
	sudo apt-get -y upgrade
	sudo apt-get install -y htop vim mc

<br>
## Create data and log directory with open permissions
<br>

	sudo mkdir -p /var/log/restapi
	sudo chmod a+rwx /var/log/restapi
	
	sudo mkdir -p /mnt/data/restapi
	sudo chmod a+rwx /mnt/data/restapi
	sudo mkdir /var/data
	sudo chmod a+rwx /var/data/
	
	sudo mkdir /var/downloads
	sudo chmod a+rwx /var/downloads/


<br><br>
***
<br><br>
## JDK 7

Remove the OpenJDK version that comes along with the AMI

	sudo apt-get purge openjdk-\* icedtea-\* icedtea6-\*
	cd  /var/downloads

On your windows machine - navigate to [Oracle JDK Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1637583.html) using Google Chrome browser.

Agree to License agreement. 

In the Chrome Browser open, `Settings -> Tools -> Developer Tools -> Network Tab`;

Now click on the the version of JDK that suits you - mine is `jdk-7u5-linux-i586.tar.gz`

In the network tab you will find `GET` request; right click on the entry and select `Copy link address`; you will find that link address has authparams.

Now go back to ec2 console;

	wget http://download.oracle.com/otn-pub/java/jdk/7u5-b05/jdk-7u5-linux-i586.tar.gz?AuthParam=xxxxx
	mv jdk* jdk-7u5-linux-i586.tar.gz
	tar -zxf jdk-7u5-linux-i586.tar.gz
	sudo mkdir /usr/lib/jvm
	sudo mv jdk1.7.0_05 /usr/lib/jvm/jdk1.7.0
	sudo ln -s /usr/lib/jvm/jdk1.7.0 /usr/lib/jvm/java-7-sun
	sudo -s
	echo 'export JAVA_HOME=/usr/lib/jvm/java-7-sun' >> /etc/bash.bashrc
	echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/bash.bashrc
	exit
	source /etc/bash.bashrc


note: if for any reason jdk-7 fails to download, copy from local mc using `secure copy - scp` command (in case you are using linux)

	scp -P 22 -i rest-server-key.pem /home/usr/downloads/jdk-7-linux-i586.tar.gz 
		ubuntu@<dns-of-mc>:/var/downloads/

<br><br>
***
<br><br>

## Tomcat 7

<br>
## Download Tomcat 7
<br>

	cd /var/downloads
	wget http://apache.favoritelinks.net/tomcat/tomcat-7/v7.0.28/bin/apache-tomcat-7.0.28.tar.gz
	tar -zxf apache-tomcat-7.0.28.tar.gz
	sudo mkdir -p /opt/env/restapi
	sudo mv apache-tomcat-7.0.28 /opt/env/restapi/tomcat

<br>
## Configure Tomcat 7
<br>
	
	cd /opt/env/restapi/tomcat/
	vim conf/server.xml
	
By default Tomcat runs under port 8080; but we haven't open the traffic on 8080 yet in EC2 AWS Console.
Lets add new traffic rule to allow all inbound traffic on port 8080 under security groups section.

You can verify Tomcat running from your browser - specify your {elasticip}:8080

<br><br>
***
<br><br>

## MySQL

First remove any exisitng MySQL on EC2

	sudo apt-get --purge remove mysql-server mysql-common mysql-client
	sudo apt-get install -y mysql-server
	start mysql

<br><br>	
***
<br><br>
