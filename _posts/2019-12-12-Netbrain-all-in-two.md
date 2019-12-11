---
layout: article
title: Netbrain all-in-two setup
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

# Database and Application servers setup

Find SW/HW requirements on [original copy](https://netbraintech.com/docs/ie80/NetBrain_System_Setup_Guide_Two-Server_Deployment.pdf)

**Version:** 8.0

## Pre-Installation Tasks

Centos version:

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


## Install Database Server

#### Login to CentOS as root

	# Create and access dir under **/opt** to download the installation package
	mkdir /opt/netbraintemp
	# Download the package
	wget http://download.netbraintech.com/netbrain-all-in-two-linux-x86_64-rhel7-8.0.tar.gz
	# Extract installation files
	tar -zxvf netbrain-all-in-two-linux-x86_64-rhel7-8.0.tar.gz
	# Access the netbrain-all-in-two-linux-8.0.1 dir
	# Run ./install.sh to install netbrain linux components
	# Accept EULA

#### Issues

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

	*"The free space in the path is less than 100GB. It may result in insufficient disk space after a period of use. Do you want to continue?*


- log parh for Netbrain **/var/log/netbrain**
- ip address of this machine **x.x.x.x**
- netbrain service name **admin**
- netbrain service password: *****