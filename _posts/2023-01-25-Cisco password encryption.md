---
layout: article
title: Cisco password encryption
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
articles:
  excerpt_type: html
tags: password encryption hash Cisco
---

<!--more-->

---

Each encryption type is assigned a number. The types of password encryption that are supported on a given device vary depending on the hardware and the version of IOS that is running on the device. 

### Common encryption types

- Type 0 - No encryption
- type 4 - SHA-256
- type 5 - MD5
- type 7 - Cisco propietary vigenere encryption
- type 8 - Password-based key derivation function 2 (PBKDF2) with a SHA-256 secret encryption
- type 9 - scrypt encryption


### Type 0

Are passwords stored without encryption in the running configuration. e.g. **enable password** 


### Type 4

Encrypted using SHA-256 hash algorithm. Default type of encryption used by **enable secret** in some versions of IOS.
Type 4 have been deprecated as of IOS 15.3(3).


### Type 5

Encrypted using an MD5 hash algorithm. Currently used for **enable secret**

If both **enable password** and **enable secret** are configured on the same device, the device will prefer the Type 5 password when a user is prompted for the enable mode password. This is likewise true of any other command that can be configured with secret keyword.


### Type 7

This is Cisco propietary encryption. It's aimed at protecting from shoulder surfing. But not really robust once you get a copy of it.

### Type 8

Password-based key derivation function 2 (PBKDF2) with SHA256 are not reversible and are considered stronger than all previous types. You can run **enable secret 8** to configure them.

To configure type 8 passwords as default secret type, issue the **enable algorithm-type sha256** in Cisco IOS later than 15.3(3)M3.


### Type 9
Encrypted in the running configuration by using the scrypt encryption algorithm. Type 9 are the strongest form of encryption available on Cisco devices. Not reversible.

To set type 9 as default secret type issue the **enable algorithm-type scrypt** in Cisco IOS later than 15.3(3)M3.

### Warning

If you plan to downgrade a device to a Cisco IOS version earlier than 15.3(3)M3 and we are using either type 8 or type 9 encryption. you should first issue the **enable algorithm-type md5** command in 15.3(3)M3 and reconfigure the **enable secret** command. Otherwise you will be locked out of the Cisco device after downgrading.




Reference: [VXLAN terminology](https://www.cisco.com/c/en/us/support/docs/switches/nexus-9000-series-switches/118978-config-vxlan-00.html#anc5)
