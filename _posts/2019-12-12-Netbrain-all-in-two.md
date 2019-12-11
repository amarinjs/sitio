---
layout: article
title: Netbrain all-in-two Installation
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

## Version IE 8.0

[Original copy](https://netbraintech.com/docs/ie80/NetBrain_System_Setup_Guide_Two-Server_Deployment.pdf)

### Pre-Installation Tasks

Centos version:

	rpm -qa centos-release


#### RabbitMQ 3rd party dependencies:

##### Socat

Socat [man page](https://linux.die.net/man/1/socat)

Look for it:

	rpm -qa | grep socat

If not installed:

	yum -y install socat


##### Logrotate

Logrotate [man page](https://linux.die.net/man/8/logrotate)

Look for it:

	rpm -qa | grep logrotate

If not installed:

	yum -y install logrotate

