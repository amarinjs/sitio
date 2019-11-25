---
layout: article
title: Control and Management plane security
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(313deg, rgba(2,0,36,1) 0%, rgba(76,76,194,1) 47%, rgba(0,212,255,1) 100%)'
    src: https://github.com/alexma2344/peperina/blob/master/assets/images/don-ripper.jpg?raw=true"
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

#### configure a SNMPv3 group

	HQ-ISR(config)# snmp-server group MY-GROUP v3 priv

#### configure a SNMPv3 user

Use [this](https://github.com/alexma2344/peperina/tree/master/docs/assets) table for reference


	HQ-ISR(config)# snmp-server user admin MY-GROUP v3 auth sha AUTHKEY123 priv aes 256 PRIVKEY123

<div>{%- include extensions/youtube.html id='wbY97-hdD5c' -%}</div>

## Control Plane Protection