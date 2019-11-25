---
layout: article
title: Control and Management plane security
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#4c4cc2'
  background_image:
    gradient: 'linear-gradient(313deg, rgba(2,0,36, .6) 0%, rgba(76,76,194, .6) 47%, rgba(0,212,255, .6) 100%)'
    src: https://github.com/alexma2344/peperina/blob/master/assets/images/rainbows.jpg?raw=true"
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

## SNMPv3 access

Before snmp config always set a contact and location

	HQ-ISR(config)#snmp-server contact Admin
	HQ-ISR(config)#snmp-server location London HQ

#### Configure a SNMPv3 group

	HQ-ISR(config)# snmp-server group MY-GROUP v3 priv

#### Configure a SNMPv3 user

Use [this](https://github.com/alexma2344/peperina/tree/master/docs/assets/snmpv3-template) table for reference


	HQ-ISR(config)# snmp-server user admin MY-GROUP v3 auth sha AUTHKEY123 priv aes 256 PRIVKEY123

#### Set an SNMP server

We define the server to which traps will be sent

	HQ-ISR(config)# snmp-server host 10.10.2.40 version 3 priv admin
	HQ-ISR(config)# snmp-server enable traps 
	HQ-ISR(config)# end

### Checks

On the device see that the user is configured, eg

	HQ-ISR# show snmp user
	
	User name: admin
	Engine ID: 800000090300B0FAEB64B388
	storage-type: nonvolatile        active
	Authentication Protocol: SHA
	Privacy Protocol: AES128
	Group-name: SECURE-X

#### MIB browsers

Use a MIB browser
- [Hilisoft](https://download.cnet.com/HiliSoft-MIB-Browser/3000-2651_4-10698289.html) 
- [paessler](https://www.paessler.com/tools/snmptester)
## Control Plane Protection