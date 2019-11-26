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
tags: cisco ios-xe asa snmp security ssh
---

<!--more-->

---

## Intro

When we open management doors to the infrastructure, attack vectors are also created. We'll first write down how to configure management access, then exploit the introduced weaknesses, and finally address them with countermeasures (control plane policing).

Here are some [best practices](https://tools.cisco.com/security/center/resources/copp_best_practices) from Cisco (more complete content).

<center><img src="https://github.com/alexma2344/sitio/blob/master/assets/images/device-planes.png?raw=true"></center>
<div style="text-align: center;">
    <span style="font-size:11px; color:grey">
        Network device planes. 
    </span>
</div>

**Note:** The examples are being done on Cisco platforms but do apply to network infrastructure in general.

## SSH access

Set a domain name and generate an RSA key pair:
	
	HQ-ISR(config)# ip domain name local.mycompany.com
	HQ-ISR(config)# crypto key generate rsa modulus 2048
	% You already have RSA keys defined named HQ-ISR.local.mycompany.com.
	% They will be replaced.
	
	% The key modulus size is 2048 bits
	% Generating 2048 bit RSA keys, keys will be non-exportable...
	[OK] (elapsed time was 1 seconds)

Enable ssh version 2 for [enhanced security](https://en.wikipedia.org/wiki/Secure_Shell#Version_2.x):

	HQ-ISR(config)# ip ssh version 2

Then allow only ssh on the virtual teletype (VTY) lines:

	HQ-ISR(config)#line vty 0 98
	HQ-ISR(config-line)#transport input ssh

**Note:** An ACL can be added to the VTY lines to specify allowed subnets, in the case of the ASA, a subnet filter is part of the standard config, eg

	asav(config)# ssh 192.168.25.0 255.255.255.0 mgmt

## SNMPv3 access

Set a contact and location

	HQ-ISR(config)#snmp-server contact Admin
	HQ-ISR(config)#snmp-server location London HQ

#### Configure a SNMPv3 group:

	HQ-ISR(config)# snmp-server group MY-GROUP v3 priv

#### Configure a SNMPv3 user: 

Specify which protocols to use for authentication and encrytion and write them down.

Use [this table](https://github.com/alexma2344/sitio/tree/master/docs/assets/snmpv3-template) for documentation.


	HQ-ISR(config)# snmp-server user admin MY-GROUP v3 auth sha AUTHKEY123 priv aes 256 PRIVKEY123

#### Set an SNMP server:

We define the server to which traps will be sent

	HQ-ISR(config)# snmp-server host 10.10.2.40 version 3 priv admin
	HQ-ISR(config)# snmp-server enable traps 
	HQ-ISR(config)# end

On the other end (Monitoring tool), use the created [table](https://github.com/alexma2344/sitio/tree/master/docs/assets/snmpv3-template) and make sure everything matches.

#### Verification

See that the user is configured

	HQ-ISR# show snmp user
	
	User name: admin
	Engine ID: 800000090300B0FEEB64B388
	storage-type: nonvolatile        active
	Authentication Protocol: SHA
	Privacy Protocol: AES256
	Group-name: MY-GROUP


From the montoring tool I would run Wireshark or tcpdump to look for snmp traffic.

**Note:** You can decrypt SNMPv3 in wireshark to see OID information in transit, [guide here](https://hi.service-now.com/kb_view.do?sysparm_article=KB0716409)

## Control Plane Protection

### Attack

Use [snmpwalk](https://linux.die.net/man/1/snmpwalk) to overflow the CPU of the device.

	root@ubuntu-home:~# snmpwalk -v3 -l authPriv -u admin -a SHA -A AUTHKEY123 -x AES -X PRIVKEY123 172.16.2.3 1


### Countermeasure

### Tools

These are some MIB browsers/snmp testers:

- [Hilisoft](https://download.cnet.com/HiliSoft-MIB-Browser/3000-2651_4-10698289.html) 
- [Paessler](https://www.paessler.com/tools/snmptester)
- [snmpwalk](https://linux.die.net/man/1/snmpwalk)