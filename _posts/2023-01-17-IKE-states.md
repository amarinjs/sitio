---
layout: article
title: IKE states
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
tags: ike security 
---

<!--more-->

---

## Overview
   The purpose is to negotiate,
   and provide authenticated keying material for, security associations
   in a protected manner.

   Processes which implement this memo can be used for negotiating
   virtual private networks (VPNs) and also for providing a remote user
   from a remote site (whose IP address need not be known beforehand)
   access to a secure host or network.

[RFC2409](https://datatracker.ietf.org/doc/html/rfc2409#section-2)


[show crypto isakmp sa](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/security/s1/sec-s1-cr-book/sec-cr-s3.html#wp5743341910)


[Discussion](https://community.cisco.com/t5/network-security/quot-show-crypto-isakmp-sa-quot-explanation/td-p/1074312#:~:text=The%20output%20of%20show%20cry,isakmp%20SA%20was%20built%20successfuly.)

When a router is performing main mode Internet Key Exchange (IKE) peering, the IKE states will appear in the output of show crypto isakmp sa command in the following order:


- MM_NO_STATE
- MM_SA_SETUP
- MM_KEY_EXCH
- MM_KEY_AUTH
- QM_IDLE

### Example

    CA-TOR-R1#show crypto isakmp sa
    IPv4 Crypto ISAKMP SA
    dst             src             state          conn-id status
    103.1.1.1       102.1.1.1       QM_IDLE           1033 ACTIVE
    101.1.1.1       102.1.1.1       QM_IDLE           1032 ACTIVE


