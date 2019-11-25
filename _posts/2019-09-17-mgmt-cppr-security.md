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
    gradient: 'linear-gradient(313deg, rgba(2,0,36, .3) 0%, rgba(76,76,194, .3) 47%, rgba(0,212,255, .6) 100%)'
    src: https://github.com/alexma2344/peperina/blob/master/assets/images/gross-clinic.jpg?raw=true"
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
	HQ-ISR(config)#snmp-server location 	London HQ

#### configure a SNMPv3 group

	HQ-ISR(config)# snmp-server group MY-GROUP v3 priv

#### configure a SNMPv3 user

Use this excel table for reference

<center><img src="https://github.com/alexma2344/peperina/blob/master/assets/images/excel-sheet.PNG?raw=true"></center>



## Control Plane Protection