---
layout: article
title: NetBrain System Security
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
tags: netbrain tenant
---

<!--more-->

---

## User Accounts

Use external authentication when possible. Otherwise, be sensible about NetBrain's password policy.

Keep in mind the **rule of least privilege** as you would with any other system.

#### Network Security

Domain Management --> Advanced settings.

## Internal Communications

### Default Protocols

- Inter-server comms are conducted via TCP
- Thin client communicates to the server via HTTP

### Traffic Hardening

- Set IIS to use HTTPS
- Set inter-server communications to use SSL during installation process
- Traffic between Front Servers and back-end is automatically encrypted (AES 128)
- Do not use Telnet

## Server Hardening

### Windows Server

- [Encrypt](https://www.howtogeek.com/192894/how-to-set-up-bitlocker-encryption-on-windows/) the hard drive
- Install AV
	But add *X:/Program Files/NetBrain* and subfolders to AV exception list
- Keep windows update running
- [Backup](https://kb.vmware.com/s/article/2006202) the server regularly (easy on a virtual env)
- Enforce security policies
- Disable the [IIS page](https://www.owasp.org/index.php/Hardening_IIS)


### CentOS / RH Linux

- Use [Encrypted Storage Engine](https://docs.mongodb.com/manual/core/security-encryption-at-rest/)
- Keep the OS [updated](https://phoenixnap.com/kb/how-to-update-upgrade-centos)
- [Backup](https://kb.vmware.com/s/article/2006202) the server regularly
- Enforce security policies
- Do not use a GUI
- Disable root login after installation
- Disable IPv6 if not in use