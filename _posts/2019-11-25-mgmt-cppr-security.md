---
layout: article
title: SNMPv3
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

When we open management doors to the infrastructure, attack vectors are also created. We'll first write down how to configure SNMPv3 access, then exploit the introduced weakness, and finally address it with countermeasures (control plane policing). 

Here an extensive guide on [control plane best practices](https://tools.cisco.com/security/center/resources/copp_best_practices) from Cisco. My excersise is just a day-to-day task that needs documenting.

<center><img src="https://github.com/alexma2344/sitio/blob/master/assets/images/device-planes.png?raw=true"></center>
<div style="text-align: center;">
    <span style="font-size:11px; color:grey">
        Network device planes. 
    </span>
</div>

**Note:** The examples are being done on Cisco platforms but do apply to network infrastructure in general.


## SNMPv3 access

Set a contact and location (optional)

	HQ-ISR(config)# snmp-server contact Admin
	HQ-ISR(config)# snmp-server location London HQ

### Configure a SNMPv3 group:

	HQ-ISR(config)# snmp-server group MY-GROUP v3 priv

### Configure a SNMPv3 user: 

Specify which protocols to use for authentication and encrytion and write them down.

Use [this table](https://github.com/alexma2344/sitio/tree/master/docs/assets/snmpv3-template) for documentation.


	HQ-ISR(config)# snmp-server user admin MY-GROUP v3 auth sha AUTHKEY123 priv aes 128 PRIVKEY123

**Note:** For some reason the MIB browsers won't be able to retrieve GET when using AES 256, when I change to AES 128 it works... (we should use 256).

### Set an SNMP server:

If you need to send traps somewhere (you can still poll without it).

	HQ-ISR(config)# snmp-server host 172.16.10.12 version 3 priv admin
	HQ-ISR(config)# snmp-server enable traps 
	HQ-ISR(config)# end

On the other end (Monitoring tool), use the created [table](https://github.com/alexma2344/sitio/tree/master/docs/assets/snmpv3-template) and make sure everything matches.

### Verification

See that the user is configured

	HQ-ISR# show snmp user
	
	User name: admin
	Engine ID: 800000090300B0FEEB64B388
	storage-type: nonvolatile        active
	Authentication Protocol: SHA
	Privacy Protocol: AES128
	Group-name: MY-GROUP


- From the montoring tool host run Wireshark or tcpdump to look for snmp traffic.

- Use an snmp tester like the one below to poll some OIDs

<center><img src="https://github.com/alexma2344/sitio/blob/master/assets/images/poll.PNG?raw=true"></center>
<div style="text-align: center;">
    <span style="font-size:11px; color:grey">
        Polling interfaces from SNMP tester.
    </span>
</div>

**Note:** You can decrypt SNMPv3 in wireshark to see OID information in transit, [guide here](https://hi.service-now.com/kb_view.do?sysparm_article=KB0716409)

#### Deleting a user

This one is confusing, I don't know why it happens, it won't allow you to delete the user bound to the original engineID

To delete it:

1. snmp-server engineid local original engineid

2. no snmp-server user username group version


## Control Plane Protection

### Attack

Attack the The control-plane host subinterface (where the snmp process resides)

Use [snmpwalk](https://linux.die.net/man/1/snmpwalk) to overflow the CPU of the device.

	root@ubuntu-home:~# snmpwalk -v3 -l authPriv -u admin -a SHA -A AUTHKEY123 -x AES -X PRIVKEY123 172.16.2.3 1


### Countermeasure

### Tools

These are some MIB browsers/snmp testers:

- [Hilisoft](https://download.cnet.com/HiliSoft-MIB-Browser/3000-2651_4-10698289.html) 
- [Paessler](https://www.paessler.com/tools/snmptester)
- [snmpwalk](https://linux.die.net/man/1/snmpwalk)