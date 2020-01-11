---
layout: article
title: Netbrain Two-Server Deployment
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#4c4cc2'
  background_image:
    gradient: 'linear-gradient(313deg, rgba(2,0,36, .6) 0%, rgba(76,76,194, .6) 47%, rgba(0,212,255, .6) 100%)'
    src: https://github.com/alexma2344/sitio/blob/master/assets/images/rainbows.jpg?raw=true"
sharing: true
comment: true
mathjax: true
mathjax_autoNumber: true
articles:
  excerpt_type: html
tags: netbrain centos mongodb redis rabbitmq elasticsearch
---

<!--more-->

---

## Preliminary checks/docs to review

Find SW/HW requirements on [original copy](https://netbraintech.com/docs/ie80/NetBrain_System_Setup_Guide_Two-Server_Deployment.pdf)

[System Setup Guide (Two-Server Deployment)](https://www.netbraintech.com/docs/ie80/NetBrain_System_Setup_Guide_Two-Server_Deployment.pdf)
[System Setup Guide (Distributed Deployment)](https://www.netbraintech.com/docs/ie80/NetBrain_System_Setup_Guide_Distributed_Deployment.pdf)

[System Upgrade Guide (for 8.0 Two-Server Deployment)](https://www.netbraintech.com/docs/ie80/NetBrain_System_Upgrade_Guide_Two-Server_Deployment_8.0.pdf)

- Server Setup (Windows/Linux 7.5/7.6/7.7) – keep in mind to have the 3rd party dependencies package for Linux downloaded (Setup Guide)
- Ping/SNMP/CLI Access (SSH/Telnet) from Windows Server
- restAPI access from Windows Server
- adjustment of ACL on devices which prevent access via Ping/SNMP/CLI/restAPI to a device
- Firewalls – need more attention to be prepared to have access ready



**NetBrain Version:** 8.0

<center><img src="https://github.com/alexma2344/sitio/blob/master/assets/images/dsa.PNG?raw=true"></center>
<div style="text-align: center;">
    <span style="font-size:11px; color:grey">
        System overview. 
    </span>
</div>

## Pre-Installation Tasks

CentOS version should be 7.6,

	rpm -qa centos-release


### RabbitMQ 3rd party dependencies:

##### Socat

[man page](https://linux.die.net/man/1/socat)


	rpm -qa | grep socat

If not installed:

	yum -y install socat


##### Logrotate

[man page](https://linux.die.net/man/8/logrotate)


	rpm -qa | grep logrotate

If not installed:

	yum -y install logrotate

### Service monitor agent 3rd party dependencies:

- package: ***zlib-devel readline-devel bzip2-devel ncurses-devel gdbm-devel xz-devel tk-devel openssl-devel libffi-devel***

Check if installed:

	rpm -qa|grep -E "zlibdevel|readline-devel|bzip2-devel|ncurses-devel|gdbm-devel|xz-devel|tk-devel|openssldevel|libffi-devel"

If not installed:

	yum -y install zlib-devel readline-devel bzip2-devel ncurses-devel gdbmdevel xz-devel tk-devel openssl-devel libffi-devel


### Numactl recommended

[man page](https://linux.die.net/man/8/numactl)

	rpm -qa|grep numactl

	yum install numactl


## Database Server

#### Login to CentOS as root

	# Create and access dir under **/opt** to download the installation package
	mkdir /opt/netbraintemp
	cd /opt/netbraintemp
	# Download the package
	wget http://download.netbraintech.com/netbrain-all-in-two-linux-x86_64-rhel7-8.0.tar.gz
	# Extract installation files
	tar -zxvf netbrain-all-in-two-linux-x86_64-rhel7-8.0.tar.gz
	# Access the netbrain-all-in-two-linux-8.0.1 dir
	# Run ./install.sh to install netbrain linux components
	# Accept EULA

### Issues

You might get errors such as:

>ERROR: To perform all's installation, following dependencies are required.
>Missing dependencies: gdbm-devel
>Please choose one of the following two options to install the dependencies.
>  1) Online install:
>      Run "yum -y install gdbm-devel" to download the dependencies online and install them.

>  2) Offline install:
>      Download the dependency package from an internet-enabled server using "http://download.netbraintech.com/dependencies-centos7.6-8.0.tar.gz".
>      Copy the downloaded dependency package from the other server to this server.
>      Run tar -zxvf dependencies-centos7.6-8.0.tar.gz to decompress the package
>      Run offline-install.sh within the decompressed directory to install the dependencies.

> Re-run the install.sh or upgrade.sh after installing all of the dependencies. The installation aborted.

Install any missing dependencies and re run **./install.sh**

#### Important parameters

- data path for NetBrain **/var/lib/netbrain**

	*The free space in the path is less than 100GB. It may result in insufficient disk space after a period of use. Do you want to continue?* --> yes, this is a demo.


- log parh for Netbrain **/var/log/netbrain**

	*The free space in the path is less than 50GB. It may result in insufficient disk space after a period of use. Do you want to continue?* --> yes, this is a demo.

- ip address of this machine **x.x.x.x** (127.0.0.1 not supported)
- netbrain service name **admin**
- netbrain service password: *****
	*These credentials will apply to all the services and the communication between them.*
- Use SSL on NetBrain Services?: **no**
- Use customized server ports?: **no**
- Please enter the URL (must end with /) to call NetBrain Web API service for the Service Monitor [http(s)://<IP address or hostname of NetBrain Application Server>/]: http://x.x.x.x/


>  Note:
> The username and password cannot contain any of the following
> special characters, and their lengths cannot exceed 64 characters.
> { } [ ] : " , ' | < > @ & ^ % \ and spaces

> Note:
> The username and password cannot be empty and it cannot start with ! or #. 


### Paths and Ports

|-----------------+------------|
| Parameter | Value |
|:-----------------|:-----------|
| Data path | /var/lib/netbrain |
| Log path     | /var/log/netbrain |
| License Agent port     | 27654 |  
| Netbrain Service Username     |  admin | 
| Netbrain Service Password 	 | password123 | 
| License Agent port     | 27654 |  
| Elasticsearch port     |  9200 | 
| RabbitMQ port          | 5672| 
| Redis port             | 6379, 7000 (SSL) | 
|-----------------+------------|


**Note:** Keep notes of the user name and password because it will be used for validating the connections with:
- MongoDB, Elasticsearch, RabbitMQ, and Redis when installing NetBrain Application Server
- Front Server Controller when setting up the system
- Service Monitor Agent when communicating with Web API Server

NetBrain Web API service URL:   http://x.x.x.x/ServicesAPI

Do you want to continue using these parameters? [yes]


> WARNING: The specified directory has less than 50GB free space, which may result in abnormal use after the Elasticsearch runs for a period time.

This being a demo environment, we can worry about free space later or when we note performance issues.

The final outputs should be:

> All the NetBrain Linux components have been installed successfully.
> Please restart the operating system to make kernel settings of MongoDB to take effect.

##### Reboot the OS

Check the following services:

	systemctl status mongod
	systemctl status netbrainlicense
	systemctl status elasticsearch
	systemctl status rabbitmq-server
	systemctl status redis
	systemctl status netbrainagent


**Note:** If you have customized any port, you must add it to the corresponding configuration file:

|-----------------+------------|
| Service Name |  File Name |
|:-----------------|:-----------|
| MongoDB | mongodb.yaml |
| License Agent | license.yaml |
| Elasticsearch | elasticsearch.yaml |
| RabbitMQ | rabbitmq.yaml |
| Redis | redis.yaml |
|-----------------+------------|

**Path:** /etc/netbrain/nbagent/checks

This is in yaml so you need to be very specific on alignment, punctuation and spaces **eg**

	init_config:
	instances:
	 - name: default
	   port: 27000

Use the above as reference.


## Application Server

- Disable IE enhanced security

To set the IP and DNS from PS:

	netsh interface ip set address name="Ethernet0" static 192.168.4.10 255.255.255.0 192.168.4.1
	netsh interface ip set dns "Ethernet0" static 192.168.1.50

Complete the following steps with administrative privileges:

1. Download the netbrain-all-in-two-windows-x86_64-8.0.zip file from
(http://download.netbraintech.com/netbrain-all-in-two-windows-x86_64-8.0.zip) and save it in your local folder.
2. Extract files from the netbrain-all-in-two-windows-x86_64-8.0.zip file.
3. Navigate to the netbrain-all-in-two-windows-x86_64-8.0 folder, right-click the netbrain-application-8.0.1.exe file
and then select Run as administrator to launch the Installation Wizard.