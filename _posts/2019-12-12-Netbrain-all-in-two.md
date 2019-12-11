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

[Original copy](https://netbraintech.com/docs/ie80/NetBrain_System_Setup_Guide_Two-Server_Deployment.pdf)

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

1. Login to CentOS as root

2. Create dir under **/opt** to download the installation package

**eg** mkdir /opt/netbraintemp

3. Download the package

**wget http://download.netbraintech.com/netbrain-all-in-two-linux-x86_64-rhel7-8.0.tar.gz**