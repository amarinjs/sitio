---
layout: article
title: Management and Control plane security
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
tags: cisco ios-xe asa snmp security
---

<!--more-->

---

## Intro

I'm writing down a cheatsheet for future reference on management and control plane security.

When we open management access to the infrastructure, attack vectors/vulnerabilities are also created. This will be addressed on the Countermeasure section to show a use case of Control Plane Policing.

Here are some [best practices](https://tools.cisco.com/security/center/resources/copp_best_practices) from Cisco.

**Note:**The examples are being done on Cisco platforms but do apply to network infrastructure in general.

## Control Plane Protection

### Attack

Use [snmpwalk](https://linux.die.net/man/1/snmpwalk) to overflow the CPU of the device.

	root@ubuntu-home:~# snmpwalk -v3 -l authPriv -u admin -a SHA -A AUTHKEY123 -x AES -X PRIVKEY123 172.16.2.3 1


### Countermeasure

## SSH access



## SNMPv3 access

Set a contact and location

	HQ-ISR(config)#snmp-server contact Admin
	HQ-ISR(config)#snmp-server location London HQ

#### Configure a SNMPv3 group

	HQ-ISR(config)# snmp-server group MY-GROUP v3 priv

#### Configure a SNMPv3 user

Use [this](https://github.com/alexma2344/sitio/tree/master/docs/assets/snmpv3-template) table for reference


	HQ-ISR(config)# snmp-server user admin MY-GROUP v3 auth sha AUTHKEY123 priv aes 256 PRIVKEY123

#### Set an SNMP server

We define the server to which traps will be sent

	HQ-ISR(config)# snmp-server host 10.10.2.40 version 3 priv admin
	HQ-ISR(config)# snmp-server enable traps 
	HQ-ISR(config)# end

#### Verification

On the device see that the user is configured

	HQ-ISR# show snmp user
	
	User name: admin
	Engine ID: 800000090300B0FEEB64B388
	storage-type: nonvolatile        active
	Authentication Protocol: SHA
	Privacy Protocol: AES256
	Group-name: MY-GROUP

#### Tools

These are some MIB browsers/snmp testers:

- [Hilisoft](https://download.cnet.com/HiliSoft-MIB-Browser/3000-2651_4-10698289.html) 
- [Paessler](https://www.paessler.com/tools/snmptester)
- [snmpwalk](https://linux.die.net/man/1/snmpwalk)